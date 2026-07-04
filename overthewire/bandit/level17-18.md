# 🚩 Bandit Level 17 → 18
**Platform:** OverTheWire — Bandit  
**Category:** Linux · File Comparison · Text Processing  
**Difficulty:** 🟡 Intermediate  

---

## 📋 Description / Descripción

**EN:** There are two files in the home directory: `passwords.old` and `passwords.new`. The password for the next level is in `passwords.new` and is the only line that has been changed between the two files.

**ES:** Hay dos archivos en el directorio home: `passwords.old` y `passwords.new`. La contraseña para el siguiente nivel está en `passwords.new` y es la única línea que ha cambiado entre los dos archivos.

---

## 🔍 Reconnaissance / Reconocimiento

Al conectarse al nivel se encuentran dos archivos con listas de contraseñas:  
*Upon connecting to the level, two files with password lists are found:*

```bash
bandit17@bandit:~$ ls -la
-rw-r----- 1 bandit18 bandit17 3300 passwords.new
-rw-r----- 1 bandit18 bandit17 3300 passwords.old
```

El objetivo es identificar **la única línea diferente** entre ambos archivos.  
*The goal is to identify the **only different line** between both files.*

---

## 🚪 Solution / Solución

### Método 1 — `diff` (más directo / most straightforward)
```bash
diff passwords.old passwords.new | grep ">" | cut -d " " -f2
```
- `diff` compara ambos archivos línea por línea
- `grep ">"` filtra solo las líneas nuevas (añadidas en passwords.new)
- `cut -d " " -f2` extrae solo la contraseña sin el símbolo `>`

---

### Método 2 — `grep -v -f` (exclusión inversa / inverse exclusion)
```bash
grep -v -f passwords.old passwords.new
```
- `-f passwords.old` usa el archivo viejo como lista de patrones
- `-v` invierte la búsqueda — muestra solo lo que NO está en passwords.old
- Resultado: la única línea nueva = la contraseña

---

### Método 3 — `comm` (comparación de columnas / column comparison)
```bash
comm -13 passwords.old passwords.new
```
- `comm` compara dos archivos ordenados
- `-13` suprime columnas 1 y 3 → muestra solo líneas únicas de passwords.new

---

### Método 4 — `awk` (procesamiento de texto / text processing)
```bash
awk 'NR==FNR{a[$0];next}!($0 in a)' passwords.old passwords.new
```
- Primera pasada: carga passwords.old en un array `a[]`
- Segunda pasada: imprime líneas de passwords.new que no están en `a[]`

---

## 🏁 Flag

```
[contraseña obtenida con cualquiera de los métodos anteriores]
```
> ⚠️ *La flag no se publica para respetar las reglas de OverTheWire.*  
> *Flag not published to respect OverTheWire rules.*

---

## 📚 Lessons Learned / Lecciones Aprendidas

**ES:**
- Existen múltiples herramientas en Linux para comparar archivos — cada una con sus ventajas
- `diff` es la más directa para comparaciones simples
- `awk` es la más poderosa y flexible para procesamiento complejo
- `comm` requiere archivos ordenados pero es muy eficiente
- Conocer varias soluciones para un mismo problema demuestra profundidad técnica

**EN:**
- *Multiple Linux tools exist for file comparison — each with its advantages*
- *`diff` is the most straightforward for simple comparisons*
- *`awk` is the most powerful and flexible for complex processing*
- *`comm` requires sorted files but is very efficient*
- *Knowing multiple solutions for the same problem demonstrates technical depth*

---

## 🛠️ Commands Reference / Referencia de Comandos

| Comando | Uso |
|---|---|
| `diff file1 file2` | Compara dos archivos línea por línea |
| `grep -v -f pattern file` | Excluye líneas que coinciden con un patrón |
| `comm -13 file1 file2` | Muestra líneas únicas del segundo archivo |
| `awk 'NR==FNR{...}'` | Compara archivos con procesamiento avanzado |
| `cut -d " " -f2` | Extrae campos de texto por delimitador |

---

## 🔗 References / Referencias

- [OverTheWire Bandit Level 17](https://overthewire.org/wargames/bandit/bandit18.html)
- [Linux diff manual](https://man7.org/linux/man-pages/man1/diff.1.html)
- [Linux awk manual](https://man7.org/linux/man-pages/man1/awk.1p.html)

---

*Write-up by Yalianna | Cybersecurity Analyst*  
*[GitHub](https://github.com/Y2208) · [LinkedIn](https://linkedin.com/in/yaliannavaldes)*
