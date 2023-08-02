1-Pour le transfert de données vers la NUR(en MySQL):

import csv

# Chemin du fichier CSV
chemin_fichier_csv = 'donnees.csv'

# Écrire les données dans le fichier CSV
with open(chemin_fichier_csv, 'w', newline='') as csvfile:
    csvwriter = csv.writer(csvfile)
    csvwriter.writerow(["colonne1", "colonne2"])  # Écrire les en-têtes des colonnes (facultatif)
    csvwriter.writerows(donnees)

2- Ajouter le système de reconnaissance faciale en prenant les visages des employés en se connectant à L'IA:

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

3- Pour Ajouter la vérification des cartes via le capteur:

# Liste des cartes d'accès autorisées
cartes_autorisees = ["1234ABCD", "5678EFGH", "9012IJKL"]

def verifier_acces(carte):
    if carte in cartes_autorisees:
        return True
    else:
        return False

# Demande à l'utilisateur de saisir le numéro de carte
numero_carte = input("Veuillez entrer le numéro de votre carte d'accès : ")

# Vérifie si l'accès est autorisé ou non
if verifier_acces(numero_carte):
    print("Accès autorisé.")
else:
    print("Accès refusé.")

4- Pour donner l'accès à d’autre personne (HG):

def verifier_acces_utilisateur(utilisateur, liste_utilisateurs_autorises):
    if utilisateur in liste_utilisateurs_autorises:
        return True
    else:
        return False

# Liste des utilisateurs autorisés à accéder à la salle
liste_utilisateurs_autorises = ['utilisateur1', 'utilisateur2', 'utilisateur3']

# Demander le nom d'utilisateur à l'utilisateur
nom_utilisateur = input("Entrez votre nom d'utilisateur : ")

# Vérifier l'accès de l'utilisateur
acces_autorise = verifier_acces_utilisateur(nom_utilisateur, liste_utilisateurs_autorises)

# Afficher le résultat
if acces_autorise:
    print("Accès autorisé. Bienvenue dans la salle.")
else:
    print("Accès refusé. Vous n'êtes pas autorisé à accéder à cette salle.")
