#FirebaseRealtime-Regras 
Aqui estão algumas regras para o firebasa database. Para começar, vamos deixar uma regra inteira e imagens do banco de dados

```{
 "rules":{
     ".read" : "auth != null && root.child('P').child(auth.uid).exists() && root.child('P').child(auth.uid).child('status').val() !='morto'",
     ".write" : "true"
   }
}```

```cookie169/Regras-Firebase```
