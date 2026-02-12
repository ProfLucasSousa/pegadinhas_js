# 🧪 Aula — Pegadinhas do JavaScript e o Design da Linguagem

## 🎯 Objetivo
Entender por que certas expressões em JavaScript retornam resultados inesperados — e o que isso revela sobre as decisões de design da linguagem.

> JS não foi feito para ser perfeito. Foi feito para rodar. O resto foi resolvido depois — e nem tudo foi resolvido.

---

# 🔎 typeof NaN

```js
typeof NaN
// "number"
```

## O que acontece
`NaN` significa *Not a Number*, mas o tipo é `number`.

## Por quê
`NaN` é um valor especial dentro do sistema numérico.

```js
0 / 0 // NaN
```

## 💬 Reflexão de design
O tipo indica a categoria interna, não a validade do valor. A linguagem classifica pela estrutura, não pelo significado.

---

# 🔢 Limite de precisão numérica

```js
9999999999999999
// 10000000000000000
```

## O que acontece
O número muda sozinho.

## Por quê
JS usa IEEE 754 (float 64 bits) para todos os números.

```js
Number.MAX_SAFE_INTEGER
```

## 💬 Reflexão de design
Um único tipo numérico simplifica a engine, mas sacrifica precisão inteira alta. Simples por dentro, surpreendente por fora.

---

# ➗ Ponto flutuante estranho

```js
0.5 + 0.1 == 0.6
// true

0.1 + 0.2 == 0.3
// false
```

```js
0.1 + 0.2
// 0.30000000000000004
```

## Por quê
Decimais não são exatos em binário.

## 💬 Reflexão de design
Isso vem do padrão de hardware. JS não tenta esconder — ele expõe. Transparente, mas perigoso pra quem confia demais.

---

# 📈 Math.max() e Math.min() vazios

```js
Math.max()
// -Infinity

Math.min()
// Infinity
```

## Por quê
São os valores base internos de comparação.

## 💬 Reflexão de design
Função sem parâmetro ainda retorna algo válido. Evita exceção, mas pode mascarar erro silencioso.

---

# ➕ Array + Array

```js
[] + []
// ""
```

## Passos
- Objetos com `+` sofrem coerção
- Array vira string
- `[].toString()` → ""

## 💬 Reflexão de design
O operador `+` forcinga string se um lado virar texto. Um operador, dois comportamentos. Prático e traiçoeiro.

---

# 📦 Array + Objeto

```js
[] + {}
// "[object Object]"
```

## Conversões

```
[] → ""
{} → "[object Object]"
```

## 💬 Reflexão de design
Todo objeto tem string padrão. Ajuda debug rápido, gera resultado estranho em expressão.

---

# 🧱 Objeto + Array

```js
{} + []
// 0   (no console)
```

## Por quê
No console `{}` vira bloco, não objeto.

```js
+[] // 0
```

Forçando objeto:

```js
({} + [])
// "[object Object]"
```

## 💬 Reflexão de design
Parsing depende do contexto. Mesma escrita, leitura diferente. A engine entende. O humano sofre.

---

# ✅ Booleanos em operações

```js
true + true + true === 3
// true
```

```js
true - true
// 0
```

## Conversão

```
true → 1
false → 0
```

## 💬 Reflexão de design
Coerção automática reduz digitação, aumenta ambiguidade. Ótimo em script curto, perigoso em regra de negócio.

---

# ⚖️ == vs ===

```js
true == 1
// true

true === 1
// false
```

## Diferença

- `==` converte tipos
- `===` compara tipo e valor

## 💬 Reflexão de design
Dois operadores para o mesmo símbolo de igualdade foi a tentativa de agradar todo mundo. Resultado: uma regra de ouro nasceu — use `===`.

---

# 🧬 Expressão maluca

```js
(!+[] + [] + ![]).length
// 9
```

## Passo a passo

```js
+[] → 0
!0 → true
![] → false
```

```js
true + [] → "true"
"true" + false → "truefalse"
```

```js
"truefalse".length → 9
```

## 💬 Reflexão de design
Encadeamento de coerção + operador sobrecarregado = truque de palco. Funciona, mas não é pra produção — a menos que você goste de caos controlado.

---

# 🔤 Número + string

```js
9 + "1"
// "91"
```

## Por quê
String presente → concatenação.

## 💬 Reflexão de design
O operador decide o modo pelo tipo do operando. Dinâmico demais para quem espera previsibilidade rígida.

---

# ➖ String − string

```js
91 - "1"
// 90
```

## Por quê
Operadores matemáticos forçam conversão numérica.

## 💬 Reflexão de design
`+` concatena, `-` converte. Mesma família de operadores, regras diferentes. Consistência não foi prioridade aqui.

---

# 📭 Array comparado com número

```js
[] == 0
// true
```

## Conversão

```
[] → ""
"" → 0
```

## 💬 Reflexão de design
Comparação frouxa tenta ajudar. Às vezes ajuda errado com convicção.

---

# 🧠 Conclusões sobre o design do JS

- Coerção automática é parte central da linguagem
- Um tipo numérico só
- Operadores sobrecarregados
- Parsing dependente de contexto
- Comparação frouxa permissiva

JS favorece conveniência rápida. O preço é a surpresa tardia.

---

# 🛡️ Regras de sobrevivência

```js
Use ===
Converta tipos explicitamente
Evite depender de coerção
Teste floats com tolerância
Prefira clareza a truque
```

> Código esperto ganha aplauso. Código claro evita reunião de emergência.

