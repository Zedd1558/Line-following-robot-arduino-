# Line-following-robot-arduino-
a line following robot code used by arduino 

#define inA 6
#define inB 5
#define inC 4
#define inD 3
#define ENA 7
#define ENB 8
#define sensorOut 18
#define setpoint 400
#define z 70
#define lsp z
#define rsp z
#define rms z+35
#define lms z+35
#define kp .5
#define kd 2.2
#define minus -100
#define setpointsonar 11


const int trigPinl = 32;
const int echoPin1 = 30;
const int trigPinr = 22;
const int echoPinr = 24;
const int trigPinf = 26 ;
const int echoPinf = 28;

int flag = 0;


//const int s0 = 14;
//const int s1 = 15;
//const int s2 = 16;
//const int s3 = 17;
//const int out = 18;
//int red = 0;
//int green = 0;
//int blue = 0;


long duration1;
int distance1;
long duration2;
int distance2;
long duration3;
int distance3;
float factor1 = 1;
float factor2 = 1;
//void colour();
//void sonarcolour();
int sonar();
void check1();
void check2();
//void check3();
int threshold[] = {500, 500, 500, 400, 500, 400, 600, 500, 500, 500, 500, 500};



const  int  number_of_sensor = 11, numPidSensor = 9;

int a[number_of_sensor];

void setup() {
  
  // put your setup code here, to run once:
 
  Serial.begin(9600);
  pinMode(inA, OUTPUT);
  pinMode(inB, OUTPUT);
  pinMode(inC, OUTPUT);
  pinMode(inD, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);
  //pinMode(s0, OUTPUT);
  //pinMode(s1, OUTPUT);
  //pinMode(s2, OUTPUT);
  //pinMode(s3, OUTPUT);
  //pinMode(out, INPUT);

  //digitalWrite(s0, HIGH);
  //digitalWrite(s1, HIGH);
  pinMode(trigPinl, OUTPUT);
  pinMode(echoPin1, INPUT);
  pinMode(trigPinr, OUTPUT);
  pinMode(echoPinr, INPUT);
  pinMode(trigPinf, OUTPUT);
  pinMode(echoPinf, INPUT);
  /* while(true)
    /*{
    motor(100,100);
    delay(180);
    motor(100,-100);
    delay(370);
    motor(0,0);
    delay(70);
    motor(100,100);
    delay(400);
    motor(100,-160);
    delay(440);
    motor(0,0);
    delay(200);

    }*/


}

void loop()
{

  // put your main code here, to run repeatedly:
  pid();
  //motor(120*factor1,120*factor2);
  //sonar();
}



int error, e, lastError = 0;

void pid()
{


  int pos = sensor();

 /* if ((a[3] == 1 && a[4] == 1 && a[5] == 1 && a[0] == 1 && a[2] == 1) || ( a[4] == 1 && a[5] == 1 && a[0] == 1 && a[2] == 1) || (a[3] == 1 && a[4] == 1  && a[0] == 1 && a[2] == 1))
  {

    check2();

  }



  if (( a[3] == 1 && a[4] == 1 && a[5] == 1 && a[0] == 1 && a[8] == 1 ) || ( a[3] == 1 && a[4] == 1  && a[0] == 1 && a[8] == 1 ) || ( a[4] == 1 && a[5] == 1 && a[0] == 1 && a[8] == 1 ))
  {
    motor(70, 70);
    delay(130);
    motor(-70, 70);
    delay(490);
    motor(-70, -70);
    delay(100);


  }
  
  if (( a[3] == 1 && a[4] == 1 && a[5] == 1  && a[8] == 1 ) || ( a[3] == 1 && a[4] == 1  && a[8] == 1 ) || ( a[4] == 1 && a[5] == 1  && a[8] == 1 ))
  {

      check1();


  }
  
  if ((  a[3] == 0 && a[5] == 0 && a[4] == 0 && a[0] == 1 && a[1] == 1 && a[2] == 1 && a[7] == 1 && a[8] == 1 && a[6] == 1) || (  a[3] == 0 && a[4] == 0 && a[0] == 1 && a[1] == 1 && a[2] == 1 && a[7] == 1 && a[8] == 1 && a[6] == 1) || (   a[5] == 0 && a[4] == 0 && a[0] == 1 && a[1] == 1 && a[2] == 1 && a[7] == 1 && a[8] == 1 && a[6] == 1))
  {

    flag = 1;

  }
  
  if ((a[0] == 0 && a[1] == 0 && a[2] == 0 && a[3] == 0 && a[4] == 0 && a[5] == 0 && a[6] == 0 && a[7] == 0 && a[8] == 0 && a[9] == 0 && a[10] == 0 ))
  {
    sonar();

  }*/
  /*if((a[3]==1 && a[4]==1 && a[5]==1 &&a[9]==1)||(a[3]==1 && a[4]==1  &&a[9]==1)|| ( a[4]==1 && a[5]==1 &&a[9]==1))
    {
    motor(0,0);
    delay(500);
    motor(-100,-100);
    delay(300);
    check1();
    }

    //if((a[3]==1 && a[4]==1 && a[5]==1 &&a[10]==1)||(a[3]==1 && a[4]==1  &&a[10]==1)|| ( a[4]==1 && a[5]==1 &&a[10]==1))
    //{
    //check3();
    //}
    if((a[0]==0 && a[1]==0 && a[2]==0 && a[3]==0 && a[4]==0 && a[5]==0 && a[6]==0 && a[7]==0 && a[8]==0 && a[9]==0 && a[10]==0 ))
    {
    motor(0,0);
    delay(500);
    motor(100,100);
    delay(200);
    motor(-100,100);
    delay(565);
    motor(0,0);
    delay(1000);


    }*/

  error = setpoint - pos;
  e = kp * error + kd * (error - lastError) ;
  lastError = error;

  //Serial.print(error);
  //Serial.println();
  int ls = lsp - e;
  int rs = rsp + e;

  if (ls > lms) ls = lms;
  if (rs > rms) rs = rms;
  if (ls < minus) ls = minus;
  if (rs < minus)rs = minus;

  motor( ls * factor1, rs * factor2);


  // Serial.println(String(error) + "\t rps" + String(rs)+"\t lpps"+String(ls));
  //motor( ls, rs);
}


void motor(int ls, int rs)
{
  if (ls > 0) {

    lmo(1, 0, ls);
  }
  else if (ls < 0)
  {
    lmo(0, 1, (-1)*ls);
  }
  else
  {
    lmo(0, 0, 0);
  }

  if (rs > 0) {
    rmo (1, 0, rs);
  }
  else if (rs < 0) {
    rmo(0, 1, (-1)*rs);
  }
  else
  {
    rmo(0, 0, 0);
  }
  //Serial.println("\t rs " + String(rs)+"\t ls"+String(ls));

}


void lmo(int a, int b, int ls)
{
  digitalWrite(inA, a);
  digitalWrite(inB, b);
  analogWrite(ENA, ls);
}

void rmo(int a, int b, int rs) 
{
  digitalWrite(inC, a);
  digitalWrite(inD, b);
  analogWrite(ENB, rs);
}





int sensor()
{
  
  int pos, neu = 0, sensorOnLine = 0;
  
    for (int i = 0; i < number_of_sensor ; i++)
    {
      //#ifdef BLACK
      a[i] = analogRead(i);

      Serial.print(a[i]);
      Serial.print('\t');
      /// Serial.println();
      if (a[i] > threshold[i])

      {
        a[i] = 1;
        //Serial.print(a[i]);
        // as pid works for 1st 5 sensor
        if (i < numPidSensor) {
          neu = neu + i;
          sensorOnLine++;
        }
      }
      else a[i] = 0;
      //Serial.print(a[i]);
      //Serial.print('\t');
      //#endif

    }
    Serial.println();
    if (sensorOnLine == 0) pos =  setpoint - lastError;
    else  (pos = (100 * neu) / sensorOnLine);

    //Serial.print(pos);
    //Serial.print("\t\t");
    return pos;
  }
 


void check1()
{
  motor(100, 100);
  delay(50);
  sensor();

  if (( a[3] == 1 && a[4] == 1 && a[5] == 1 && a[0] == 1 && a[8] == 1 ) || (  a[4] == 1 && a[5] == 1 && a[0] == 1 && a[8] == 1 ) || ( a[3] == 1 && a[4] == 1  && a[0] == 1 && a[8] == 1 ))
  {

    motor(70, 70);
    delay(130);
    motor(-70, 70);
    delay(490);
    motor(-70, -70);
    delay(100);



  }
  else   if ((  a[3] == 0 && a[5] == 0 && a[4] == 0 && a[0] == 1 && a[1] == 1 && a[2] == 1 && a[7] == 1 && a[8] == 1 && a[6] == 1) || (  a[3] == 0 && a[4] == 0 && a[0] == 1 && a[1] == 1 && a[2] == 1 && a[7] == 1 && a[8] == 1 && a[6] == 1) || (   a[5] == 0 && a[4] == 0 && a[0] == 1 && a[1] == 1 && a[2] == 1 && a[7] == 1 && a[8] == 1 && a[6] == 1))
  {
    flag = 1  ;
  }

  else
  {
    motor(100, 100);
    delay(50);
    motor(70, -70);
    delay(450);


  }
}
void check2()
{
  motor(100, 100);
  delay(50);
  sensor();
  if (( a[3] == 1 && a[4] == 1 && a[5] == 1 && a[0] == 1 && a[8] == 1 ) || (  a[4] == 1 && a[5] == 1 && a[0] == 1 && a[8] == 1 ) || ( a[3] == 1 && a[4] == 1  && a[0] == 1 && a[8] == 1 ))
  {
    motor(70, 70);
    delay(130);
    motor(-70, 70);
    delay(490);
    motor(-70, -70);
    delay(100);
  }
  else if ((  a[3] == 0 && a[5] == 0 && a[4] == 0 && a[0] == 1 && a[1] == 1 && a[2] == 1 && a[7] == 1 && a[8] == 1 && a[6] == 1) || (  a[3] == 0 && a[4] == 0 && a[0] == 1 && a[1] == 1 && a[2] == 1 && a[7] == 1 && a[8] == 1 && a[6] == 1) || (   a[5] == 0 && a[4] == 0 && a[0] == 1 && a[1] == 1 && a[2] == 1 && a[7] == 1 && a[8] == 1 && a[6] == 1))
  {

    flag = 1;

  }
  else
  {
    motor(100, 100);
    delay(50);
    motor(-70, 70);
    delay(450);
    motor(-70, -70);
    delay(100);
  }


}

