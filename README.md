import cv2
import dlib

# Chargement du détecteur de visage
detector = dlib.get_frontal_face_detector()

# Chargement du modèle pour la détection des points de repère faciaux
predictor = dlib.shape_predictor("chemin/vers/shape_predictor_68_face_landmarks.dat")

# Fonction pour détecter les visages et extraire les points de repère
def detect_faces_landmarks(image_path):
    image = cv2.imread(image_path)
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

    # Détection des visages
    faces = detector(gray)

    face_landmarks = []
    for face in faces:
        # Extraction des points de repère faciaux
        landmarks = predictor(gray, face)
        face_landmarks.append(landmarks)

    return faces, face_landmarks

# Exemple d'utilisation
image_path = "chemin/vers/image.jpg"
faces, face_landmarks = detect_faces_landmarks(image_path)

# Afficher les visages détectés et les points de repère
for face, landmarks in zip(faces, face_landmarks):
    left, top, right, bottom = face.left(), face.top(), face.right(), face.bottom()
    cv2.rectangle(image, (left, top), (right, bottom), (0, 255, 0), 2)

    for i in range(68):
        x, y = landmarks.part(i).x, landmarks.part(i).y
        cv2.circle(image, (x, y), 1, (0, 0, 255), -1)

cv2.imshow("Facial Landmarks", image)
cv2.waitKey(0)
cv2.destroyAllWindows()
