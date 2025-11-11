<img width="358" height="348" alt="image" src="https://github.com/user-attachments/assets/41e6350e-4482-4b53-88b3-6cc512389fb6" />



void setup() {


  pinMode(2, INPUT); 
  pinMode(3, OUTPUT); 

}


void loop(){
  if (digitalRead(2) == HIGH){  
    digitalWrite(3, HIGH);    
  }
  else {
    digitalWrite(3, LOW); 
  }
}
