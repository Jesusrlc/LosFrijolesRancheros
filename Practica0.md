![Captura de pantalla 2024-04-29 155122](https://gist.github.com/assets/158230496/93e1751a-a456-4204-b510-5ca4a3cebfa4)

![efdb](https://gist.github.com/assets/158230496/69cfa19d-7c85-4c1d-9762-121e470859b8)

```cpp
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);  // turn the LED on (HIGH is the voltage level)
  delay(1000);                      // wait for a second
  digitalWrite(LED_BUILTIN, LOW);   // turn the LED off by making the voltage LOW
  delay(1000);                      // wait for a second
}

