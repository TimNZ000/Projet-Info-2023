# Projet-Info-2023
import random
Timothée


map = [[0,0,0,1,1],[0,0,0,0,1],[1,1,0,0,0],[0,0,0,0,0]]
dico = {0:' ',1:'#'}
#p = {"char": "o", "x": 1, "y": 1}
def display_map(map,dico):
   for i in range (len(map)):
       for j in range (len(map[0])):
           #on n'est pas à la fin de la ligne
           if j!=len(map[0])-1:
               print(dico[map[i][j]],end="")
           #on est à la fin de la ligne
           else:
               print(dico[map[i][j]])

def get_random_int(max_map):#choisi un nombre aléatoire pour mes pièce
   n = random.randint(0, max_map)
   #print(f"random value is {n}")
   return n

def is_empty(x,y,m):#si la position est vide
   if m[y][x]==0:
       #print("C'est un vide")
       return True
   else:
       return False


def create_objects(nb_objects,m):
   resultat=[]
   for i in range(0,nb_objects):
       #print(i)
       x=get_random_int(len(map[0])-1)# on calcule de manière aléatoire le x de la pièce
       y=get_random_int(len(map)-1)# ".." y de la pièce
       print(f" x :{x}")
       print(f" y :{y}")
       # on regarde maintenant si la position sur la carte est libre
       if is_empty(x,y,m):#si c'est vide on ajoute le couple de position pour l'emplacement de la pièce
           resultat.append((x,y))

   return resultat



def create_perso (depart):#coordonnée de départ de mon perso
   return {"char": "o", "x": depart[0], "y": depart[1]}
#resultat=create_perso((0,0))
#print(resultat)

def display_map_and_char(m, d, p):#affiche la map avec mon perso
  for i in range(len(m)):#parcours les lignes
      for j in range(len(m[0])):#parcours les colonnes
          if (j, i) == (p["x"], p["y"]):#si les coordonnées corresponde à ,celle du personnage
              print(p["char"], end="")#on ajoute l'élément à la bonne position
          else:#sinon, elle affiche la carte
              if j != len(m[0]) - 1:
                  print(d[m[i][j]], end="")
              else:
                  print(d[m[i][j]])


def display_map_char_and_objects(m, d, p,objects):
  for i in range(len(m)):#parcours les lignes
      for j in range(len(m[0])):#parcours les colonnes
          if (j, i) == (p["x"], p["y"]):#si les coordonnées corresponde à ,celle du personnage
              print(p["char"], end="")#on ajoute l'élément à la bonne position
          else:#sinon, elle affiche la carte
               if (j,i) in objects:
                   if  j != len(m[0]) - 1:
                       print("$",end="")
                   else:
                       print("$")
               else:
                  if j != len(m[0]) - 1:
                      print(d[m[i][j]], end="")
                  else:
                      print(d[m[i][j]])              
#display_map_and_char(map, dico, create_perso((0,0)))

def is_wall(p,m):#si y'a un mur
   if m[p["y"]][p["x"]]==1:
       print("C'est un mur")
       return True
   else:
       return False


def update_p(letter, p, m,objects):
   # La valeur maximale pour l'axe y (nombre de lignes) on fait -1 pour avoir le dernier element 
   max_y = len(m) - 1  
# La valeur maximale pour l'axe x (nombre de colonnes)
   max_x = len(m[0]) - 1  
   # la condition que j'ai rajouté en plus c'est que l'axe des y > 0 

   if letter == "z" and p["y"] > 0:
       p["y"] -= 1  # Déplacement vers le haut
       if is_wall(p,m): #si c'est un mur, on revient à la position d'avant
           p["y"] += 1
   elif letter == "q" and p["x"] > 0:
       p["x"] -= 1  # Déplacement vers la gauche
       if is_wall(p,m):
           p["x"] += 1
   elif letter == "s" and p["y"] < max_y:
       p["y"] += 1  # Déplacement vers le bas
       if is_wall(p,m):
           p["y"] -= 1
   elif letter == "d" and p["x"] < max_x:
       p["x"] += 1  # Déplacement vers la droite
       if is_wall(p,m):
           p["x"] -= 1
   objects=update_objects(p,objects)
   return objects

def update_objects(p,objects):#mon perso mange la pièce
   if (p["x"],p["y"]) in objects:
       print("piece")
       objects.remove((p["x"],p["y"]))
   return objects


objects=create_objects(4,map)
#display_map(map,dico)
p=create_perso((0,0))
user_input ="Y"
display_map_char_and_objects(map, dico, p,objects)#affiche ma map avec mon pion à sa possition initiale
while user_input!="Q" :
# Affichage initial
# Récupération de l'entrée utilisateur
   user_input = input("Quel déplacement ? ")#choisi le déplacement

# Mise à jour de la position du joueur
   objects=update_p(user_input, p, map,objects)

# Affichage après mise à jour
   display_map_char_and_objects(map, dico, p,objects)
