# hdntCenter-V2.0

# 20092601
1、新发现bug，曲线设置会导致异常死机，未解决，今天太晚了。明天解决。
2、更新了协议库，发现协议库底层做的有点不好，后续重新搭建，采用显式调用方式应更为合理。

# 20092002
1、增加曲线保存图片和捕捉曲线功能。
2、修正另存为存储数据时覆盖相同文件名报错问题。

# 20092001
1、更新曲线绘制库，修正之前一版，曲线开多内存占用过高问题

# 20091501
update hdntGPCon
update user mannual

# 使用说明
if you want to use long time, please send the serial number to liuning1898@hotmail.com

如需开放特定功能入口，可联系邮件：liuning1898@hotmail.com

Provide access to open specific functions, contact email: liuning1898@hotmail.com

the imu and ins data solve software.

The transport protocols is:

IEEE 754
Header: 0x55 0xaa
Length: all the data length 1 byte
Contents: float (IEEE 754) ; 1 float/4 bytes
check: all the bytes sum unsigned char.

the send function as follow.

int ProtIEEE754(float *FloatDat,int FloatLen,unsigned char *buffer)
{


      //unsigned char buffer[255]={0};
      unsigned char  check=0;   //crc check
      unsigned int count=0;   //transmit data length
      unsigned int length=0;
      unsigned int i=0;
      unsigned char *Send_P;

      float temp[100];

      for(i=0;i<FloatLen;i++){
    	  temp[i]=FloatDat[i];
      }

      buffer[count]=0x55;check=check+buffer[count];count++;
      buffer[count]=0xAA;check=check+buffer[count];count++;

      /*header 2 + length 1 +crc 1*/
      buffer[count]=FloatLen*4+4;check=check+buffer[count];count++;
      /*send the data*/
      Send_P = (unsigned char *) &temp;
      for(i=0;i<FloatLen*4;i++)
      {
        buffer[count]=(unsigned char ) *Send_P;
        check=check+*Send_P;
        Send_P++;count++;
      }

      /*send the check*/
      buffer[count]=(unsigned char) check;count++;
      length=count;
      return length;
}
