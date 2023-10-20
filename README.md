# PP-SPD

// C++ code
//Definimos cada pin con su respectivo elemento

#define SWITCH 4
#define PULSADOR_SUBE 3
#define PULSADOR_BAJA 2
#define UNIDAD A0
#define DECENA A1
#define A 10
#define B 11
#define C 5
#define D 6
#define E 7
#define F 9
#define G 8
#define OFF 0
#define FUERZA A2
#define FOTORESISTENCIA A3

//Declaramos variables
int contador = 0;
int sube = 1;
int sube_antes = 1;
int baja = 1;
int baja_antes = 1;

//Declaramos la función que cumple cada elemento 
void setup()
{
    pinMode(SWITCH, INPUT_PULLUP);
    pinMode(PULSADOR_SUBE, INPUT_PULLUP);
    pinMode(PULSADOR_BAJA, INPUT_PULLUP);
    pinMode(A, OUTPUT);
    pinMode(B, OUTPUT);
    pinMode(C, OUTPUT);
    pinMode(D, OUTPUT);
    pinMode(E, OUTPUT);
    pinMode(F, OUTPUT);
    pinMode(G, OUTPUT);
    pinMode(FUERZA,INPUT);
  	pinMode(UNIDAD, OUTPUT);
  	pinMode(DECENA, OUTPUT);
    pinMode(FOTORESISTENCIA, INPUT);
    digitalWrite(UNIDAD, 0);
  	digitalWrite(DECENA, 0);
  	display_numeros(0);
    Serial.begin(9600);
}


void loop()
 { 
  //Declaramos las variables que vamos a usar
  int estado_switch = digitalRead(SWITCH) ;
  int apretado = boton_apretado();
  int sensor_fuerza = analogRead(FUERZA);
  int valor_fotoresistencia = analogRead(FOTORESISTENCIA);


  
  /* Si la fuerza que detecta el sensor es mayor a 4N
  aproximadamente el contador vuelve a 0*/
  if(sensor_fuerza > 80){
    contador = 0;
    }
  int brillo = map(FOTORESISTENCIA, 0, 1023, 0, 255);
  analogWrite(UNIDAD, brillo); 
  delay(100); 

 /*Evalua si el switch esta del lado derecho, y se ejecuta el 
  contador, disminuyendo o aumentando el numero, 
  dependiendo del pulsador seleccionado */
  if(estado_switch == 0){
    
	if(apretado == PULSADOR_SUBE){
    	contador++;
      	if(contador > 99){
        	contador = 0;
        }
    }
  	else if(apretado == PULSADOR_BAJA)
    {
    	contador--;
      	if(contador < 0){
        	contador = 99;
        }
    }
    
   }
   /*Si el switch esta del lado izquierdo, va a mostrar solo los
   numeros primos, en orden creiente o decreciente 
   segun el pulsador seleccionado */
  else{
    int divisores_totales = 0;
    
    if(apretado == PULSADOR_SUBE){
    	contador++;
      for(int i = 1; i <=contador; i++){ 
      	if (contador% i == 0){ 
          divisores_totales++;
       	 }
      }
      /*Si el numero tiene mas de dos divisores, o uno solo,
      y por lo tanto no es primo, el contador aumenta para mostar
      el siguiente numero primo */
      while (divisores_totales == 1 || divisores_totales > 2){
        if(apretado == PULSADOR_SUBE){
          contador++;
          }
      	if(contador > 97){
        	contador = 0;
         }
        divisores_totales = 0;
        for(int i = 1; i <=contador; i++){ 
      	if (contador% i == 0){ 
          divisores_totales++;
       	 }
      }
        
    }
       }
    else if(apretado == PULSADOR_BAJA){
    	contador--;
      
      for(int i = 1; i <=contador; i++){ 
      	if (contador% i == 0){ 
          divisores_totales++;
       	 }
      }
      while ( divisores_totales > 2 || divisores_totales == 1){
        if(apretado == PULSADOR_BAJA){
          contador--;
        }
         if(contador < 0){
           	contador = 97;
          }
        divisores_totales = 0;
        for(int i = 1; i <=contador; i++){ 
      		if (contador% i == 0){ 
          		divisores_totales++;
       	 	}
     	 }
        
      }
 
   		}
  
  }
  
  display_contador(contador);

}

void display_numeros(int numero)
  	//Dependiendo del pulsador apretado el display muestra
  	//el valor correspondiente 
	{
  
	digitalWrite(A, HIGH);
    digitalWrite(B, HIGH);
    digitalWrite(C, HIGH);
    digitalWrite(D, HIGH);
    digitalWrite(E, HIGH);
    digitalWrite(F, HIGH);
    digitalWrite(G, HIGH);
  
    switch(numero){
      case 0:
        digitalWrite(A, LOW);
        digitalWrite(B, LOW);
        digitalWrite(C, LOW);
        digitalWrite(D, LOW);
        digitalWrite(E, LOW);
        digitalWrite(F, LOW);
        break;
      case 1:
        digitalWrite(B, LOW);
        digitalWrite(C, LOW);
        break;
      case 2:
        digitalWrite(A, LOW);
        digitalWrite(B, LOW);
        digitalWrite(E, LOW);
        digitalWrite(D, LOW);
        digitalWrite(G, LOW);
        break;
      case 3:
        digitalWrite(A, LOW);
        digitalWrite(B, LOW);
        digitalWrite(C, LOW);
        digitalWrite(D, LOW);
        digitalWrite(G, LOW);
        break;
      case 4:
        digitalWrite(B, LOW);
        digitalWrite(C, LOW);
        digitalWrite(F, LOW);
        digitalWrite(G, LOW);
        break;
      case 5:
        digitalWrite(A, LOW);
        digitalWrite(C, LOW);
        digitalWrite(D, LOW);
        digitalWrite(F, LOW);
        digitalWrite(G, LOW);
        break;
      case 6:
        digitalWrite(A, LOW);
        digitalWrite(C, LOW);
        digitalWrite(D, LOW);
        digitalWrite(E, LOW);
        digitalWrite(F, LOW);
        digitalWrite(G, LOW);
        break;
      case 7:
        digitalWrite(A, LOW);
        digitalWrite(B, LOW);
        digitalWrite(C, LOW);
        break;
      case 8:
        digitalWrite(A, LOW);
        digitalWrite(B, LOW);
        digitalWrite(C, LOW);
        digitalWrite(D, LOW);
        digitalWrite(E, LOW);
        digitalWrite(F, LOW);
      	digitalWrite(G, LOW);
        break;
      case 9:
        digitalWrite(A, LOW);
        digitalWrite(B, LOW);
        digitalWrite(C, LOW);
        digitalWrite(F, LOW);
        digitalWrite(G, LOW);
        break;
    }
}

void switch_On(int numero)
  	//Indica que display cambia dependiendo si el contador
  	//modifica a la unidad o a la decena
	{

  	
	if(numero == UNIDAD)
    {
    	digitalWrite(UNIDAD, HIGH);
      	digitalWrite(DECENA, LOW);
      	delay(10);

    }
  	else if(numero == DECENA)
    {
    	digitalWrite(UNIDAD, LOW);
      	digitalWrite(DECENA, HIGH);
      	delay(10);

    }
  	else
    {
    	digitalWrite(UNIDAD, LOW);
      	digitalWrite(DECENA, LOW);

    }
}

void display_contador(int contador)
  	//Muestra el número en el display dependiendo si el contador
  	//sube o disminuye
	{
  
	switch_On(OFF);
  	display_numeros(contador/10);
  	switch_On(DECENA);
  	switch_On(OFF);
  	display_numeros(contador - 10 * ((int)contador / 10));
  	switch_On(UNIDAD);
	}

int boton_apretado(void)
  //declara cada botón pulsado
{

	sube = digitalRead(PULSADOR_SUBE);
  	baja = digitalRead(PULSADOR_BAJA);
  
  	
  	if(sube){
    	sube_antes = 1;  
    }
   	if(baja){
    	baja_antes = 1;
    }
   
  
  	
  	if(sube == 0 && sube != sube_antes)
    {
    	sube_antes = sube;
      	return PULSADOR_SUBE;
    }
  	if(baja == 0 && baja != baja_antes)
    {
    	baja_antes = baja;
      	return PULSADOR_BAJA;
    }
 
  
 return 0; 
   }
![imagen arduino parcial SPD](https://github.com/Ana7787/PP-SPD/assets/123705131/e574a2fd-440d-4304-939d-2a6801de1454)
