# 🚩 TryHackMe — Jr Penetration Tester Write-up
**Plataforma / Platform:** TryHackMe  
**Wargame:** Jr Penetration Tester  
**Progreso / Progress:** In Progress Offensive Security Intro
**Categoría / Category:**  · SSH · Bash · Networking · Cryptography  

---

## 📋 Índice / Index

| Nivel / Level | Concepto clave / Key Concept |
|---|---|
| [Track 1](#track-1) | Think link a Hacker!  |
| [Track 2](#track-2) | Starting de Lab|
| [Track 3](#track-3) | Find Hidden Page |
| [Track 4](#track-4) | Attack the Admin Page |


---

## Track 1
**Concepto:** Think link a Hacker! 
**Leccion:** Offensive Security is about thinking like an attacker to find weaknesses before real hackers do.

In this room, you'll hack your first website in a safe and legal environment to see how ethical hackers operate. 
Answer the questions below

Which term describes simulating a hacker's actions to find system vulnerabilities?

**Solution:** `offensive Security`


## Track 2
**Concepto:** Starting de Lab
**Leccion:** This room uses a virtual desktop to simulate a real system. Press the "View Site" button below to get started.

A fake banking application called FakeBank will launch. When the lab loads, you'll see the banking application running in your browser.
Answer the questions below

What is the bank account number shown in the FakeBank application?

**Solution:** `8881`


## Track 3
**Concepto:** Find Hidden Page
**Leccion:** Now we will find a weakness in the FakeBank website. Click the button below to get started with this portion of the room.

After clicking the above "View Site" button, we will begin using the terminal. The terminal is used to interact with the device and cybersecurity tools.

One common mistake websites make is leaving hidden pages accessible. We'll use the terminal to run a command that can look for these.

Inside the terminal, copy and paste the dirb command below and wait for it to finish. Any lines from the output that start with + are pages that have been found.

dirb http://fakebank.thm

Dirb will find two URLs. Use this information to answer the question below.
Answer the questions below

Dirb found one URL, http://fakebank.thm/images.
What is the other hidden URL?

**Herramientas:** `dirb`

**Code:** ```bash
                    dirb http://fakebank.thm
              ```
              
 **Solution:**`http://fakebank.thm/bank-transfer`


## Track 4
**Concepto:** Attack the Admin Page
**Leccion:** You should now have found a hidden admin panel that lets you add money to your account. Click the "View Site" button below to complete this section.

To naviate to this hidden admin panel, we will add this newly discovered page to the search bar located at the top within the website. To do so, you will need to add the following: /bank-transfer

Select the account number 8881 and deposit $2000 (or more). After clicking "Deposit Money", a flag will popup. Use this flag to answer the question below.
Answer the questions below

When your balance turns positive, a pop-up with green text appears.

 Enter the green words as the answer (ALL CAPS)

**Solution:** `BANK-HACKED`

---

## 📊 Resumen de habilidades / Skills Summary

| Área | Herramientas aprendidas |
|---|---|
| **Linux fundamentals** | `dirb` |
| **Security concepts** | Penetration Tester |

---

## 📚 Lecciones generales / General Lessons

- **EN:** Each level builds on the previous one. Linux proficiency is fundamental for any cybersecurity role.
- **ES:** Cada nivel construye sobre el anterior. El dominio de Linux es fundamental para cualquier rol en ciberseguridad.

- **EN:** There is almost always more than one way to solve a problem — knowing multiple approaches is a valuable skill.
- **ES:** Casi siempre hay más de una forma de resolver un problema — conocer múltiples enfoques es una habilidad valiosa.

---

*Write-up by Yalianna | Cybersecurity Analyst*  
*[GitHub](https://github.com/Y2208) · [LinkedIn](https://linkedin.com/in/yaliannavaldes)*
