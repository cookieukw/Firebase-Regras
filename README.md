#FirebaseRealtime-Regras 


![](https://user-images.githubusercontent.com/65344982/105611622-28be4300-5d95-11eb-8aed-47a3981a8589.jpg)

Aqui estão algumas regras para o firebasa database. Para começar, vamos deixar uma regra inteira e imagens do banco de dados

```
 {"rules":{
     ".read" : "auth != null && root.child('P').child(auth.uid).exists() && root.child('P').child(auth.uid).child('status').val() !='morto'",
     ".write" : "true"
   }
}
```

Agora vamos ver como funciona cada parte dessas regras

```
auth != null
```

Verifica se o user está logado

```
root.child('P').child(auth.uid).exists()
```

Verifica se  a uid do usuario existe na key "P"

```
root.child('P').child(auth.uid).child('status').val() != 'morto'"
```

Verifica se o status do usuarios esta igual "morto"

```
&&
```

Tem o mesmo valor que "e",ou seja,para o usuario poder ler algo ,ele precisa estar logado,ter a sua uid na aba de usuarios(P) e nao pode ter o valor "morto" na key "status"
