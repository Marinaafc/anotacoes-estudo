```javascript 
console.log("")
// O console.log mostra uma mensagem
```
- Exibe um valor na tela
- // = comentário

```javascript 
let nome = "Guilherme";
let idade = 20;
let idade = 10;
let nome = marina;
const idade = 20;
```
- const - atribui uma variável que não pode ser alterada
```javascript 
let nome = marina;
console.log("Olá,", nome);
console.log("Olá, " + nome);
let nome = rodrigo;
```
- vai exibir "marina", porque o código é executado por ordem de linha
- toda const precisa ter um valor inicial declarado, mas o let não precisa e se não declarar um valor no let e imprimir, aparece undefined
- ** - potencia (elevado a...)
- % - restante da divisão
- quando soma texto com numero, o resultado é texto
- typeof variavel - mostra o tipo da variável
- === operador de igualdade
- !== é diferente de
- == é parecido com
```javascript 
const condição = true;
if(condição){
  console.log("É verdade!");
} else {
console.log("É falso!");
}

console.log("acabou");
```
- && - e
- ! - ex: !temCarteirinha = não tem carteirinha
- || - ou

```javascript 
if(idade < 18 || idade > 60 || (ehAdulta && temCarteirinha)) {
...
}
```
```javascript 
for(const elemento of coleção);
}
```
- está declarando uma variável dentro da expressão
- se a variável for declarada dentro do laço, não vai ser possível usá-la fora da repetição
