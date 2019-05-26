# Project---Pet-Feeder

PIR sensor And Buzzer Sensor 
bool alertedin;

void alertin();


// PIR motion detection is running off interrupt functions, which once triggered
// will run either "alertout" or "alertin" functions. Pins D1 and D2 were
// selected as signal inputs from the PIR sensors.
void setup() {
    pinMode(D1, INPUT);
    pinMode(D4, OUTPUT);

    attachInterrupt(D1, alertin, RISING);

    alertedin = false;

}

// publishes an event to Particle Console when either the outside or
// inside PIR sensor is triggered
void loop() {
 
    if (alertedin == true) {
            Particle.publish("MotionIn","active",21600,PUBLIC);
            digitalWrite(D4, HIGH);
            delay(1000);
            digitalWrite(D4, LOW);
          //Particle.publish("MotionIn",String(Time.now()),21600,PUBLIC);
            alertedin = false;
            
    
    }
}

// sets booleans to true to tigger publish event in running loop


void alertin() {
    alertedin = true;
}


Servo Motor

// subscribes the receiver Photon to the published events from the
// mesauring Photon. In the last arguement for the subscribe function, enter
// the device ID from the publishing Photon.
Servo myservo;  // create servo object to control a servo
                // a maximum of eight servo objects can be created

int pos = 0;    // variable to store the servo position
void setup() {
    


    myservo.attach(D0);   // attach the servo on the D0 pin to the servo object
    
    pinMode(D7, OUTPUT);  // set D7 as an output so we can flash the onboard LED


   
    Particle.subscribe("MotionIn", b,"24001d000847373336323230");
   
}
// converts Unix time data from subscribe functions to an integer value

void b(const char *event, const char *data) {
     
        {
            myservo.write(0);       // move servo to 25
            digitalWrite(D7, HIGH); // flash the LED
            delay(2000);             // wait 100 ms
            myservo.write(90);      // move servo to 180°
            digitalWrite(D7, LOW);  // turn off LED
            delay(1000);            // wait 1 second 
        }
        
    }
