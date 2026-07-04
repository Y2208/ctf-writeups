# 🚩 OverTheWire — Bandit Write-up
**Plataforma / Platform:** OverTheWire  
**Wargame:** Bandit  
**Progreso / Progress:** Nivel 0 → 21  
**Categoría / Category:** Linux · SSH · Bash · Networking · Cryptography  

---

## 📋 Índice / Index

| Nivel / Level | Concepto clave / Key Concept |
|---|---|
| [0 → 1](#nivel-0--1) | Conexión SSH |
| [1 → 2](#nivel-1--2) | Leer archivo básico |
| [2 → 3](#nivel-2--3) | Archivos con guión en el nombre |
| [3 → 4](#nivel-3--4) | Archivos con espacios en el nombre |
| [4 → 5](#nivel-4--5) | Archivos ocultos |
| [5 → 6](#nivel-5--6) | Encontrar archivo por tipo |
| [6 → 7](#nivel-6--7) | Encontrar archivo por propiedades |
| [7 → 8](#nivel-7--8) | Buscar en archivos grandes |
| [8 → 9](#nivel-8--9) | Línea única en archivo |
| [9 → 10](#nivel-9--10) | Cadenas legibles en binario |
| [10 → 11](#nivel-10--11) | Decodificación Base64 |
| [11 → 12](#nivel-11--12) | Cifrado ROT13 |
| [12 → 13](#nivel-12--13) | Archivos comprimidos múltiples |
| [13 → 14](#nivel-13--14) | Autenticación con clave SSH privada |
| [14 → 15](#nivel-14--15) | Comunicación por puerto (Netcat) |
| [15 → 16](#nivel-15--16) | Comunicación SSL/TLS |
| [16 → 17](#nivel-16--17) | Escaneo de puertos SSL |
| [17 → 18 ⭐](#nivel-17--18-) | Comparación de archivos |
| [18 → 19](#nivel-18--19) | SSH ejecutando comandos remotos |
| [19 → 20](#nivel-19--20) | Binario SUID |
| [20 → 21](#nivel-20--21) | Conexión TCP con Netcat |

---

## Nivel 0 → 1
**Concepto:** Conexión SSH básica  
**Credentials:** `bandit0:bandit0`

```bash
ssh -p 2220 bandit0@bandit.labs.overthewire.org
```

**Lección:** SSH es el protocolo estándar para conexiones remotas seguras. El puerto por defecto es 22, pero OverTheWire usa el 2220.

---

## Nivel 1 → 2
**Concepto:** Leer un archivo básico  
**Herramientas:** `ls`, `cat`

```bash
ls
cat readme
```

**Lección:** `ls` lista archivos del directorio actual. `cat` muestra el contenido de un archivo.

---

## Nivel 2 → 3
**Concepto:** Archivos con guión (`-`) en el nombre  
**Herramientas:** `cat`

```bash
cat ./-
```

**Lección:** Un archivo llamado `-` es interpretado por bash como stdin. Usar `./` antes del nombre indica que es un archivo en el directorio actual, no una opción de comando.

---

## Nivel 3 → 4
**Concepto:** Archivos con espacios en el nombre  
**Herramientas:** `cat`

```bash
cat ./--spaces\ in\ this\ filename--
```

**Lección:** Los espacios en nombres de archivo requieren escaparse con `\` o encerrarse entre comillas. El autocompletado con `Tab` ayuda a evitar errores.

---

## Nivel 4 → 5
**Concepto:** Archivos ocultos en un directorio  
**Herramientas:** `cd`, `ls`, `cat`

```bash
cd ~/inhere
ll
cat ./...Hiding-From-You
```

**Lección:** Los archivos que empiezan con `.` están ocultos. `ll` (alias de `ls -la`) los muestra todos incluyendo los ocultos.

---

## Nivel 5 → 6
**Concepto:** Encontrar archivo legible entre varios  
**Herramientas:** `cat`, `head`

```bash
cat head ./-file00
```

**Lección:** Cuando hay múltiples archivos, `file` permite identificar el tipo de cada uno. Solo el archivo legible contiene texto ASCII — los demás son binarios.

---

## Nivel 6 → 7
**Concepto:** Encontrar archivo por tamaño exacto  
**Herramientas:** `find`

```bash
find -size 1033c
cd ~/inhere/maybehere07/
cat ./.file2
```

**Lección:** `find -size 1033c` busca archivos de exactamente 1033 bytes (`c` = bytes). Es una forma eficiente de filtrar entre cientos de archivos.

---

## Nivel 7 → 8
**Concepto:** Buscar archivo por usuario, grupo y tamaño en todo el sistema  
**Herramientas:** `find`

```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
```

**Lección:** `find` puede filtrar por múltiples propiedades simultáneamente. `2>/dev/null` elimina los errores de permisos del output, mostrando solo resultados útiles.

---

## Nivel 8 → 9
**Concepto:** Encontrar la única línea que no se repite  
**Herramientas:** `sort`, `uniq`

```bash
sort datos.txt | uniq -u
```

**Lección:** `sort` ordena el archivo (necesario para que `uniq` funcione correctamente). `uniq -u` muestra solo las líneas que aparecen una única vez.

---

## Nivel 9 → 10
**Concepto:** Extraer cadenas legibles de un archivo binario  
**Herramientas:** `strings`, `cat`

```bash
strings data.txt
cat data.txt 2>/dev/null | strings
```

**Lección:** `strings` extrae secuencias de caracteres legibles de archivos binarios. Útil en análisis forense y reversing básico.

---

## Nivel 10 → 11
**Concepto:** Decodificación Base64  
**Herramientas:** `echo`, `base64`

```bash
echo "VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==" | base64 -d
```

**Lección:** Base64 es un esquema de codificación común para transmitir datos binarios como texto. `-d` decodifica el string. No es cifrado — solo codificación.

---

## Nivel 11 → 12
**Concepto:** Cifrado ROT13  
**Herramientas:** `echo`, `tr`

```bash
echo "Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**Lección:** ROT13 es un cifrado de sustitución que rota cada letra 13 posiciones. `tr` (translate) reemplaza caracteres según un mapeo definido. Es el cifrado más simple — aplicarlo dos veces devuelve el texto original.

---

## Nivel 12 → 13
**Concepto:** Archivo comprimido múltiples veces  
**Herramientas:** `mktemp`, `xxd`, `file`, `gzip`, `bzip2`, `tar`

```bash
# Crear directorio temporal
mktemp -d
cd /tmp/tmp.L4I1mrVGwL
cp ~/data.txt .

# Convertir hexdump a binario
xxd -r data.txt > archivo1

# Identificar tipo y descomprimir en ciclo
file archivo1

# Ciclo de descompresión (gzip → bzip2 → gzip → tar → tar → bzip2 → tar → texto)
mv archivo1 archivo1.gz && gzip -d archivo1.gz
mv archivo1 archivo1.bz2 && bzip2 -d archivo1.bz2
mv archivo1 archivo1.tar && tar -xf archivo1.tar
```

**Lección:** `xxd -r` convierte un hexdump de vuelta a binario. `file` identifica el tipo real de un archivo independientemente de su extensión. Es importante siempre verificar el tipo antes de intentar descomprimir.

---

## Nivel 13 → 14
**Concepto:** Autenticación SSH con clave privada  
**Herramientas:** `scp`, `chmod`, `ssh`

```bash
# Copiar la clave privada a la máquina local
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .

# Dar permisos correctos (SSH rechaza claves con permisos abiertos)
chmod 600 sshkey.private

# Autenticarse con la clave privada
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220

# Obtener la contraseña del nivel siguiente
cat /etc/bandit_pass/bandit14
```

**Lección:** SSH requiere que las claves privadas tengan permisos `600` (solo lectura/escritura del propietario). `scp` permite copiar archivos de forma segura entre hosts remotos y locales.

---

## Nivel 14 → 15
**Concepto:** Comunicación por puerto con Netcat  
**Herramientas:** `nc`

```bash
nc localhost 30000
```

**Lección:** Netcat (`nc`) es la navaja suiza de las redes — permite conectarse a cualquier puerto TCP/UDP. Al enviar la contraseña actual al puerto 30000, el servidor responde con la del siguiente nivel.

---

## Nivel 15 → 16
**Concepto:** Comunicación sobre SSL/TLS  
**Herramientas:** `openssl`, `cat`

```bash
cat /etc/bandit_pass/bandit15
openssl s_client -connect localhost:30001
```

**Lección:** Cuando un puerto requiere SSL/TLS, `nc` no es suficiente. `openssl s_client` establece una conexión cifrada. Al conectarse se envía la contraseña actual y el servidor responde con la siguiente.

---

## Nivel 16 → 17
**Concepto:** Escaneo de puertos SSL para encontrar el correcto  
**Herramientas:** `openssl`, `for` loop, `timeout`

```bash
# Probar todos los puertos posibles en el rango
for port in 31046 31518 31691 31790 31960; do
    echo "Probando puerto $port"
    timeout 2 openssl s_client -connect localhost:$port 2>&1 | head -5
    echo "---"
done

# Conectarse al puerto correcto para obtener la clave privada SSH
# Copiar la private key en la PC local y acceder como bandit17
```

**Lección:** No siempre se sabe qué puerto usar. Un loop `for` con `timeout` permite probar múltiples puertos eficientemente. El puerto correcto devuelve una clave SSH privada en lugar de texto plano.

---

## Nivel 17 → 18 ⭐
**Concepto:** Comparación de archivos para encontrar la diferencia  
**Herramientas:** `diff`, `grep`, `comm`, `awk`

Este fue el nivel más desafiante — existen múltiples soluciones válidas:

```bash
# Método 1 — diff (más directo)
diff passwords.old passwords.new | grep ">" | cut -d " " -f2

# Método 2 — grep inverso
grep -v -f passwords.old passwords.new

# Método 3 — comm
comm -13 passwords.old passwords.new

# Método 4 — awk
awk 'NR==FNR{a[$0];next}!($0 in a)' passwords.old passwords.new
```

**Lección:** Hay múltiples herramientas para comparar archivos en Linux. Conocer varias alternativas es señal de madurez técnica. `diff` es el más directo; `awk` el más flexible para casos complejos.

---

## Nivel 18 → 19
**Concepto:** SSH ejecutando un comando remoto directamente  
**Herramientas:** `ssh`

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
```

**Lección:** El `.bashrc` de bandit18 cierra la sesión inmediatamente al conectarse. La solución es pasar el comando directamente como argumento a SSH — este se ejecuta antes de que el shell interactivo inicie.

---

## Nivel 19 → 20
**Concepto:** Binario con permisos SUID  
**Herramientas:** SUID binary

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

**Lección:** Un binario SUID se ejecuta con los permisos de su propietario, no del usuario que lo ejecuta. `bandit20-do` pertenece a bandit20, por lo que permite leer archivos de ese usuario.

---

## Nivel 20 → 21
**Concepto:** Comunicación TCP con Netcat en background  
**Herramientas:** `nc`, `echo`, `&`

```bash
# Levantar un listener en background que envíe la contraseña actual
echo "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 19968 &

# El binario se conecta al listener y devuelve la contraseña del siguiente nivel
./bandit20-do
```

**Lección:** El operador `&` ejecuta un proceso en background. `nc -l` escucha en un puerto específico. El binario verifica que recibe la contraseña correcta y responde con la del siguiente nivel.

---

## 📊 Resumen de habilidades / Skills Summary

| Área | Herramientas aprendidas |
|---|---|
| **Linux fundamentals** | `ls`, `cat`, `find`, `cd`, `chmod` |
| **Text processing** | `grep`, `sort`, `uniq`, `strings`, `awk`, `tr`, `cut` |
| **Encoding/Crypto** | `base64`, ROT13, `xxd` |
| **Compression** | `gzip`, `bzip2`, `tar` |
| **Networking** | `ssh`, `nc`, `openssl`, `scp` |
| **Security concepts** | SUID, SSL/TLS, SSH keys, port scanning |

---

## 📚 Lecciones generales / General Lessons

- **EN:** Each level builds on the previous one. Linux proficiency is fundamental for any cybersecurity role.
- **ES:** Cada nivel construye sobre el anterior. El dominio de Linux es fundamental para cualquier rol en ciberseguridad.

- **EN:** There is almost always more than one way to solve a problem — knowing multiple approaches is a valuable skill.
- **ES:** Casi siempre hay más de una forma de resolver un problema — conocer múltiples enfoques es una habilidad valiosa.

- **EN:** Reading `man` pages and documentation is an essential habit in security work.
- **ES:** Leer las páginas `man` y la documentación es un hábito esencial en el trabajo de seguridad.

---

*Write-up by Yalianna | Cybersecurity Analyst*  
*[GitHub](https://github.com/TU-USUARIO) · [LinkedIn](https://linkedin.com/in/TU-PERFIL)*
