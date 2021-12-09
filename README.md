#FirebaseRealtime-Regras 

Aqui estão algumas regras para o firebase database. Para começar, vamos deixar uma regra inteira e imagens do banco de dados em que as regras irão ser usadas



![fb](https://user-images.githubusercoent.com/65344982/145453337-baf96824-2ea5-4360-9e5c-7fe959940c2f.png)



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

⬆️Verifica se o user está logado



```
root.child('P').child(auth.uid).exists()
```

⬆️Verifica se  a uid do usuario existe na key "P"



```
root.child('P').child(auth.uid).child('status').val() != 'morto'"
```

⬆️Verifica se a key "status" do usuario não está igual "morto"



```
&&
```

⬆️Tem o mesmo valor que "e",ou seja,para o usuario poder ler algo ,ele precisa estar logado,ter a sua uid na aba de usuarios(P) e nao pode ter o valor "morto" na key "status"

Quanto a regra de escrever, está aberta e qualquer pessoa pode escrever dados sem logar-se ou qualquer tipo de autenticação
