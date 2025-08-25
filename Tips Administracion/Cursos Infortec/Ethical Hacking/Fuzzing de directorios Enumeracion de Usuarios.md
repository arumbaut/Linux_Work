Fuzzing de directorios
DIRB
dirb http

gobuster

FUFF
ffuf -u url -w /usr/wordlist/file

ffuf -u url -w -d ''username=name & password=sfsdf '' -w users.txt  -H ''Content-Type: ''/usr/wordlist/file