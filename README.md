# Aprendendo Visão Computacional
Estudo de aulas relacionadas a tópicos em visão computacional.

## Detecção de cores
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

O canal com o qual mais teremos contato é o _Hue_, conforme mostrado na figura acima, porque ele conterá a informação necessária das cores em nossas imagens. Se olharmos para o ciloindro de cima, veremps po seguinte:

<img width="642" height="638" alt="image" src="https://github.com/user-attachments/assets/937b7bde-90fc-4477-948d-46f6e2087a9a" />

Esta imagem representa como o valor do canal _Hue_ muda ao longo da circunfrência do cilindro, para representar diferentes cores.__

Nesse sentido, o que iremos fazer é dizer para que o código detecte todos os pixels de uma cor dada para ser analisada. Por exemplo, a cor amarela.

Além disso, não podemos indicar apenas um valor para ser detectado, precisamos indicar um intervalo de valores de cor que estamos interessados. Então, solicitamos oa programa que nos retorne todos os pixels dentro do intervalo dado, conforme idealizado na figura abaixo:

<img width="791" height="711" alt="image" src="https://github.com/user-attachments/assets/6bfdf587-4f1c-4d14-bcc6-21b98d9a4b53" />

Código completo:
````
import cv2
from PIL import Image
from util import get_limits

yellow = [0, 255, 255] #define a cor que você quer rastrear
vermelho = [0, 0, 225] #define a cor que você quer rastrear
azul = [255, 0, 0]

cap = cv2.VideoCapture(0) #define a camera que você quer usar, no caso a webcam do notebook

while True:
    ret, frame = cap.read() #le a imagem da camera  

    hsvImage = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV) #converte a imagem para HSV

    lowerLimit, upperLimit = get_limits(color=vermelho) #chama a função que define os limites da cor que você quer rastrear

    mask = cv2.inRange(hsvImage, lowerLimit, upperLimit) #cria uma mascara com os limites definidos

    mask_ = Image.fromarray(mask) #converte a mascara para uma imagem PIL

    bbox = mask_.getbbox() #pega o bounding box da mascara

    if bbox is not None: #se o bounding box não for None, ou seja, se a cor foi encontrada na imagem
        x1, y1, x2, y2 = bbox
        frame = cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 255, 0), 5) #desenha um retângulo verde ao redor do objeto detectado

    print(bbox)

    cv2.imshow('frame', frame) #mostra a imagem da camera

    if cv2.waitKey(1) & 0xFF == ord('q'): #se apertar a tecla q, o programa fecha
        break

cap.release()
cv2.destroyAllWindows()
````
## Detecção de libras com keypoints
Instale os seguintes módulos:
````
 pip install scikit-learn mediapipe
````
