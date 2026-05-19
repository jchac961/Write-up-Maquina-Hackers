# Write-up-Maquina-Hackers

Maquina de la pplataforma www.whoami-labs.com

Primeramente inicie la maquina vulnerable

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 212917" src="https://github.com/user-attachments/assets/91caf883-9091-4509-854d-556464659f76" />

# RECONOCIMIENTO

Una vez iniciada la maquina realice un escaneo de puertos utilizando la herramienta nmap, obteniendo como resultado los puertos #21, #22, #80, #8080

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 212947" src="https://github.com/user-attachments/assets/1c69693a-a8a2-44f9-b19a-06b393ef3fcf" />

Ya que obtuve estos puertos abiertos lo que hice fue verificar en un navegador web a ver que contenian los puertos #80 y el #8080

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 213024" src="https://github.com/user-attachments/assets/46292e94-170d-461e-a42f-7557e0083677" />

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 231605" src="https://github.com/user-attachments/assets/fab30c57-4aba-45db-83d7-6a1af93ce939" />

Me llamo la atención una parte que encontre donde me decia que literal la respuesta del ctf se encontraba en las imagenes

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 213041" src="https://github.com/user-attachments/assets/2ab511b7-5422-489d-a96f-5b330b0d7410" />

Ingrese al link que habia que dice " Enter Dark Web Portal" a ver a donde me llebaba

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 213054" src="https://github.com/user-attachments/assets/0023ba9a-0c3a-4b69-928b-7934c87af702" />

Verifique el codigo en busqueda de informacion importante, pero no encontre nada 


<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 214442" src="https://github.com/user-attachments/assets/010aebcb-59f3-404f-9011-417ae557c923" />

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 214455" src="https://github.com/user-attachments/assets/bfe7aa7d-e3db-4789-ab48-751dc97d7da3" />

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 214505" src="https://github.com/user-attachments/assets/b3a3e0e1-6885-4de5-b4fb-e2e76490c14c" />

Como en el mensaje me hablaba de algo de imagen busque en el codigo a ver si encontraba algo relacionado con alguna imagen y encotnre algo, asi que intente buscar con exiftool datos importantes, pero fue en vano antes de esto la decodifique ya que estaba en base64

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 215912" src="https://github.com/user-attachments/assets/eaff3b27-5b8e-4e06-a1d3-3495c8e2366b" />

# EXPLOTACION

Luego de esto pase a con los nombres que habia publicados en la pagina, realizar un diccionario para luego hacer fuerza bruta con el a ver si de esta manera tenia suerte

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 221929" src="https://github.com/user-attachments/assets/983cc3d3-5b31-40f3-ab0a-88dd62dabba0" />

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 222409" src="https://github.com/user-attachments/assets/9440894d-3785-4529-9bd5-254e0c5a38b3" />

Luego de haberlo creado me fui a hacer un ataque de fuerza bruta utilizando la herramienta hydra, obteniendo un resultado positivo

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 224236" src="https://github.com/user-attachments/assets/45c86228-e244-478b-9c78-9f12557872e8" />

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 224155" src="https://github.com/user-attachments/assets/e946aef5-fa02-4a54-a09a-0439daf6558c" />

Ya teniendo estas credenciales procedi a tratar de ingresar por medio de ssh y obtuve acceso a la maquina

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 224405" src="https://github.com/user-attachments/assets/90a0e48c-9260-44ec-ac0d-aeb2d10c90f7" />

# ESCALADA DE PRIVILEGIOS

Ya dentro de la maquina lo proximo fue buscar los binarios a ver si alguno contenia el suid activo y en efecto estaba uno llamado chmod, que por ningun motivo deberia estar ahi ya que este lo que hace es que permite cambiar los permisos de los archivos y de esta manera escalar privilegios, que es lo que haremos a continuacion

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 224453" src="https://github.com/user-attachments/assets/3b377190-89e1-450a-b3a4-4ec00a807b3c" />

Ya teniendo este binario activo lo que hice fue cambiarle los permisos al archivo /etc/passwd el cual nos permite eliminar la x de los permisos, que lo quehace es que le dice al sistema que el usuario no posee contraseña

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 224701" src="https://github.com/user-attachments/assets/6862ae4a-5d85-440e-86c5-931319df8319" />

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 224723" src="https://github.com/user-attachments/assets/e4c85fec-3fd6-4cc1-bb19-a33555fea446" />

Ya habiendo hecho esto solo queda escalar los pivilegios utilizando el comando su root y como elimine la x del archivo de passwd lo que hace es que el sistema me da acceso sin ingresar ninguna contraseña

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 224814" src="https://github.com/user-attachments/assets/41046bbe-06d2-4b01-9f14-d974a102f86f" />

# RESULTADOS

Luego de esto me dirigi a buscar la flag y me lleve la sorpresa de que se encontraba comprimida

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 224918" src="https://github.com/user-attachments/assets/2abe9e8f-f96e-4d0d-b0b1-6d909188e4e1" />

La descomprimi pero en esta ocasión lo hizo en formato .jpg

<img width="1920" height="181" alt="Captura de pantalla 2026-05-18 225417" src="https://github.com/user-attachments/assets/7b460313-801b-43aa-b1bb-ebae2d7c0199" />

Me sorprendi un poco ya que nunca habia visto algo como esto en un laboratorio, asi que fui a investigar un poco sobre que seria los pasos a seguir o como poder encontrar la flag, que lo mas seguro es qu debia estar dentro de la imagen y talvez habia que hacer algo de esteganografia

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 230432" src="https://github.com/user-attachments/assets/29fcc9fe-4687-44ab-994b-0e55ba11e406" />

Busque informacion de los metadatos de la imagen pero no encontre nada importante, asi que lo ue segui fue descargar la herramienta stegseek qiue permite hacer fuerza bruta a los archivos utilizando la esteganografia; una vez descargada la herramienta procedi a realizar el ataque

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 230905" src="https://github.com/user-attachments/assets/6aa293bf-3116-4b62-ae75-bc5a7ba0515e" />

Luego liste el contenido del archivo y me percate a simpke vista que se encontraba cifrado utilizando base 64, asi que procedi a decodificarlo

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 231001" src="https://github.com/user-attachments/assets/8ba0dbd7-a04c-4813-b08e-5c3d5c6db631" />

Y asi pude descrifrar el contenido en texto plano

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 231122" src="https://github.com/user-attachments/assets/57de9d26-e54e-49f9-97c0-435c09b45aed" />

# *POWNED*

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-18 231146" src="https://github.com/user-attachments/assets/c664ba07-ed33-40ab-a42d-1fac611ea2c2" />




