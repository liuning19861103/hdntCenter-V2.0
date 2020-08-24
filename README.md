# hdntCenter-V2.0
if you want to use long time, please send the serial number to liuning1898@hotmail.com


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
