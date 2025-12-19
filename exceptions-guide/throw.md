

## 🏐 ANALOGIA COM VÔLEI PARA **THROW**

### 🧠 Pense assim:

### 🔹 A exceção é a **bola**

### 🔹 `throw` é o **ato de sacar ou levantar a bola**

### 🔹 `catch` é **quem recebe a bola**

### 🔹 O erro 500 é o **juiz apitando ponto**

---

## 📍 Situação 1 — SEM try/catch (bola corre livre)

🎾 (bola lançada)

* O jogador **saca a bola** (`throw`)
* Ninguém tenta pegar
* A bola atravessa a quadra
* O juiz vê
* 👉 **Ponto / erro 500**

📌 Tradução técnica:

* Exceção acontece
* Ninguém captura
* Framework captura
* Vira 500

---

## 📍 Situação 2 — try/catch SEM throw (bola é segurada)

🤲 (jogador recebe a bola)

* Alguém **recebe a bola**
* **Segura**
* Não devolve
* O jogo continua

📌 Tradução técnica:

* Exceção lançada
* `catch` captura
* **Não relança**
* Erro é engolido
* ❌ Sem 500
* ❌ Teste não vê erro

---

## 📍 Situação 3 — try/catch COM throw (recebe e levanta de novo)

🤲 ➡️ 🏐

* Jogador **recebe**
* Levanta a bola de novo (`throw`)
* A bola volta a voar
* O juiz vê
* 👉 **Ponto / erro 500**

📌 Tradução técnica:

* Exceção capturada
* Relançada (`throw`)
* Framework vê
* Vira 500

---

## 🏆 MORAL DA ANALOGIA

> **A bola (exceção) só para se alguém segurar.
> Se ninguém segurar, o juiz (framework) apita o erro 500.**

---

## 🧠 Tradução direta para o seu dia a dia

* `throw` = **levantar a bola**
* `catch` sem `throw` = **segurar a bola**
* Teste unitário precisa que a bola **chegue até o juiz**
* Você só usa `throw` quando alguém segurou a bola

---

## 💬 Frase para memorizar

> **Em teste unitário, não segure a bola.
> Deixe o erro voar até o juiz.**


