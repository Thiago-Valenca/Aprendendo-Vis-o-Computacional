# Aprendendo Visão Computacional
Estudo de aulas relacionadas a tópicos em visão computacional.

Atualize o pip e instale as dependências necessárias:
````
python.exe -m pip install --upgrade pip
pip install opencv-python==4.6.0.66 numpy==1.23.4 Pillow==9.2.0
````
Estrutura básica da visão computacional, que apenas abre sua webcam:
````
import cv2

cap = cv2.VideoCapture(0) #define a camera que você quer usar, no caso a webcam do notebook

while True:
    ret, frame = cap.read() #le a imagem da camera  

    cv2.imshow('frame', frame) #mostra a imagem da camera

    if cv2.waitKey(1) & 0xFF == ord('q'): #se apertar a tecla q, o programa fecha
        break

cap.release()
cv2.destroyAllWindows()
````

Esquema das cores em HSV:


<img width="1016" height="750" alt="image" src="https://github.com/user-attachments/assets/0990c78b-a1c6-465d-8fa3-cc10a5979cbe" />

Normalmente as imagens em que trabalhamos estão em um _colorspace_ RGB ou BGR, o que significa que cada pixel é uma combinação das cores azul, verde e vermelho. Mas, em alguns casos, pode ser muito conveniente converter nosso _colorspace_ de RGB (nosso espaço "original") para outro _colorspace_, o que depende da aplicação ou objetivo que queremos lacançar. Para a detecção de cores, o _colorspace_ HSV é muito conveniente.____ __

