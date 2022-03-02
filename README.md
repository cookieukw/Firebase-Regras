#FirebaseRealtime-Regras 

Aqui estão algumas regras para o firebase database. Para começar, vamos deixar uma regra inteira e imagens do banco de dados em que as regras irão ser usadas



![IMG_20211209_152047](https://user-images.githubusercontent.com/65344982/145453538-eeb317b6-7dcc-44d1-89aa-6f37605a6420.png)


```
 {"rules":{
     ".read" : "auth != null && root.child('P').child(auth.uid).exists() && root.child('P').child(auth.uid).child('status').val() !='bloqueado'",
     ".write" : "true"
   }
}
```


Agora vamos ver como funciona cada parte dessas regras



<h1>Verifica se o user está logado</h1>

```
auth != null
```



<h1>Verifica se  a uid do usuario existe na key "P"</h1>

```
root.child('P').child(auth.uid).exists()
```


<h1>Verifica se a key "status" do usuario não está igual "bloqueado"</h1>

```
root.child('P').child(auth.uid).child('status').val() != 'bloqueado'"
```

<h1>
Tem o mesmo valor que "e",ou seja,para o usuario poder ler algo ,ele precisa estar logado,ter a sua uid na aba de usuarios(P) e nao pode ter o valor "morto" na key "status"

Quanto a regra de escrever, está aberta e qualquer pessoa pode escrever dados sem logar-se ou qualquer tipo de autenticação
</h1>

```
&&
```

<h1> Agora, vamos aprender a como verificar dados, nossas regras agora são:</h1>

```
 {"rules":{
     ".read" : false,
     "P": {
     "read" : "auth != null && root.child('P').child(auth.uid).exists() && root.child('P').child(auth.uid).child('status').val() !='bloqueado'",
     ".write" : "true",
     ".validate": newData.hasChildren(['','',''])
   }}
}
```

<h1>Agora temos o ````.validate.```, ele é uma regra assim como write e read, a função dele é  justamente fazer a validação dos dados. Nós também acrescentamos o child "P" e movemos a regra de read para ela. Então, todas as regras que entrarem dentro de "P" servirá apenas para esse child. Já as nossas regras de antes, deixam as regras como globais independente dos childs.</h1>

```
/**
Regras Globais
**/

 {"rules":{
     ".read" : "auth != null && root.child('P').child(auth.uid).exists() && root.child('P').child(auth.uid).child('status').val() !='bloqueado'",
     ".write" : "true"
   }
}
```

```
/**
Regras que funcionam apenas no child "P" enquanto que a leitura do nosso banco de dados base é false(não precisa disso para impedir a leitura do banco de dados base)
**/

{"rules":{
     ".read" : false,
     "P": {
     "read" : "auth != null && root.child('P').child(auth.uid).exists() && root.child('P').child(auth.uid).child('status').val() !='bloqueado'",
     ".write" : "true",
     ".validate": newData.hasChildren(['','',''])
   }}
}
```

<h1>Agora como nós acrescentamos um child e movemos as regras para ele, vamos fazer algumas alterações na regra validate.
Você pode notar que ali dentro da regra tem 3 locais para colocar, isso é uma array e pode variar de acordo com seu servidor. vamos preencher elas com nossas chaves</h1>

```
/** 
Antes
**/

 ".validate": newData.hasChildren(['','',''])
 
```

```
/**
Depois
**/

  ".validate": newData.hasChildren(['nome','status','user'])
 
```

<h1>Então, a partir de agora so podemos gravar dados no child "P" se ele houver as chaves "nome","status" e "user".
Agora, vamos criar outro child dentro de "P". O nome pode ser qualquer um, e lembre-se de acrescentar "$" antes do nome. Isso significa que estamos nos referindo aos childs dentro de "P", que no caso são "uid1" e "uid2"(e as regras dentro desse child se referem apenas a esses childs)</h1>

```
/**
Regras que funcionam apenas no child "P" enquanto que a leitura do nosso banco de dados base é false(não precisa disso para impedir a leitura do banco de dados base)
**/

{"rules":{
     ".read" : false,
     "P": {
     "read" : "auth != null && root.child('P').child(auth.uid).exists() && root.child('P').child(auth.uid).child('status').val() !='bloqueado'",
     ".write" : "true",
     /**
     $uids equivalem aos childs uid1 e uid2 e seus sub childs
     **/
     "$uids":{
     ".validate": newData.hasChildren(['nome','status','user'])
  }
   }
    }
}
```

<h1>Nas regras acima, estamos validandos os dados que estao em Base/P/UID/, ou seja:</h1>

```
uid1 
 Esses dados aqui em baixo(as chaves ,no caso)
   --- nome
   --- status
   --- user
uid2
   --- nome
   --- status
   --- user
```
<h1>Agora, como iremos fazer para verificar o valor de uma dessas chaves? como por exemplo, verificar se o child "uid" dentro de P/uid1 corresponde exatamente a sua uid? 
Neste caso, usando nossas regras atualmente você pode postar exatamente como deve ser nas regras mas ainda é posssível postar dados como se fosse outro usuário e isso não seria legal. Isso é simples de consertar. nNas regras, basta usar a variável $uids. lembra que eu disse que isso seria basicamente os childs dentro de "P"?
Então vamos fazer assim: </h1>

```
/**
Regras que funcionam apenas no child "P" enquanto que a leitura do nosso banco de dados base é false(não precisa disso para impedir a leitura do banco de dados base)
**/

{"rules":{
     ".read" : false,
     "P": {
     "read" : "auth != null && root.child('P').child(auth.uid).exists() && root.child('P').child(auth.uid).child('status').val() !='bloqueado'",
     ".write" : "true",
     /**
     $uids equivalem aos childs uid1 e uid2 e seus sub childs
     **/
     "$uids":{
     ".write"; "auth.uid == $uids"
     /**
     Ou seja, estamos comparando a nossa id com o child do usuário
     **/
     
     ".validate": newData.hasChildren(['nome','status','user'])
  }
   }
    }
}
```

<h1>Mas... e se tivermos um child chamado "post" e os childs dentro deles forem gerados aleatoriamente? já que se fosse uma única id, os dados atuais dela seriam trocados pelos novos

Isso também é simples de resolver

Usamos ```newData``` como função justamente para verificar dados novos, você também pode usar ```data``` mas logicamente, essa função verifica os dados já existentes no banco de dados e usamos child para pegar o child de dentro .</h1>

```

{"rules":{
     ".read" : false,
     "P": {},
     
   "posts":{
   ".validate": "newData.child('uid') == auth.uid"
   }
    }
}
/**
Agora essa regra verifica se o valor de uid na hora de criar os dados é a mesma uid de quem está postando. Porém, é necessário adicionar a chave uid se for gravar dados na hora de fazer um post. Lembrando que isso agora não é mais no child "P" e sim no child "post"

**/
```

<h1>Chegamos no fim e as nossas regras ficaram assim:</h1>

```
/**
Regras que funcionam apenas no child "P" enquanto que a leitura do nosso banco de dados base é false(não precisa disso para impedir a leitura do banco de dados base)
**/

{"rules":{
     ".read" : false,
     /**
     Child dos perfis de usuários
     **/
     
     "P": {
     "read" : "auth != null && root.child('P').child(auth.uid).exists() && root.child('P').child(auth.uid).child('status').val() !='bloqueado'",
     ".write" : "true",
     /**
     $uids equivalem aos childs uid1 e uid2 e seus sub childs
     **/
     "$uids":{
     ".write"; "auth.uid == $uids"
     /**
     Ou seja, estamos comparando a nossa id com o child do usuário
     **/
     
     ".validate": newData.hasChildren(['nome','status','user'])
  }
   },
    /**
     Child dos posts dos usuários
     **/
     
    "posts":{
    "$posts":{
   ".validate": "newData.child('uid') == auth.uid"
   }}
    }
}
```
