import random

class PlayerRPG:
    merchant_inventory = {
        "potion_regen": 3,
        "potion_force": 3,
        "potion_defense": 3
    }

    def __init__(self):
        self.classe = 0
        self.strength = 0
        self.defense = 0
        self.HP = 0
        self.XP = 0
        self.x = 3
        self.y = 3
        self.inventory = {
            "potion_regen": 1,
            "potion_force": 0,
            "potion_defense": 1,
            "super potion": 0,
            "PO": 0
        }
        self.move = {
            "basic_hit": [20, 100],
            "je te shoot si t'es chauve": [200, 10]
        }

    def map_init(self):
        list_obj = []

        for coffre in range(0, 10):
            list_obj.append("coffre")

        for monstre_facile in range(0, 10):
            list_obj.append("goblin")

        for monstre_difficile in range(0, 4):
            list_obj.append("dragon")

        for monstre_moyen in range(0, 6):
            list_obj.append("ogre")

        for feu in range(0, 4):
            list_obj.append("feu de camp")

        list_obj.append("the final")
        list_obj.append("piège - 50pv")

        for super_potion in range(0, 3):
            list_obj.append("super potion")

        for voleur in range(0, 6):
            list_obj.append("voleur de divers")

        for piège in range(0, 3):
            list_obj.append("piège")

        random.shuffle(list_obj)
        nb_colonne = 7
        game_map = [list_obj[i:i + nb_colonne] for i in range(0, len(list_obj), nb_colonne)]
        game_map[3].insert(3, "mid")

        for ligne in game_map:
            print(ligne)

        self.game_map = game_map
        return self.game_map

    def perform_move(self):
        print("vous etes a cet endroit {}".format(self.game_map[self.x][self.y]))
        deplacement = input("ou voulez-vous aller ? D pour droite G pour gauche, H pour haut et B pour bas")

        if deplacement == "G" and self.y > 0:
            self.y -= 1
            print("Vous êtes désormais à : {}".format(self.game_map[self.x][self.y]))
        elif deplacement == "D" and self.y < len(self.game_map[0]) - 1:
            self.y += 1
            print("Vous êtes désormais à : {}".format(self.game_map[self.x][self.y]))
        elif deplacement == "H" and self.x > 0:
            self.x -= 1
            print("Vous êtes désormais à : {}".format(self.game_map[self.x][self.y]))
        elif deplacement == "B" and self.x < len(self.game_map) - 1:
            self.x += 1
            print("Vous êtes désormais à : {}".format(self.game_map[self.x][self.y]))
        else:
            print("Déplacement non valide.")
        return self.game_map[self.x][self.y]

    def player(self):
        print("Choisir entre Guerrier, Archer et Tank")
        classe_choice = input()

        if classe_choice == "Guerrier":
            self.classe = "Guerrier"
            self.strength = 20
            self.defense = 20
            self.HP = 100
            self.XP = 0
            print("Votre classe est {} avec force = {}, defense = {} et PV = {}".format(self.classe, self.strength,
                                                                                        self.defense, self.HP))
        elif classe_choice == "Archer":
            self.classe = "Archer"
            self.strength = 25
            self.defense = 15
            self.HP = 100
            self.XP = 0
            print("Votre classe est {} avec force = {}, defense = {} et PV = {}".format(self.classe, self.strength,
                                                                                        self.defense, self.HP))
        elif classe_choice == "Tank":
            self.classe = "Tank"
            self.strength = 15
            self.defense = 25
            self.HP = 100
            self.XP = 0
            print("Votre classe est {} avec force = {}, defense = {} et PV = {}".format(self.classe, self.strength,
                                                                                        self.defense, self.HP))
        else:
            print("Erreur choisir entre Guerrier, Archer et Tank")

    def inventory_use(self):
        print(self.inventory)
        oui_non = input("utiliser un objet ?")
        if oui_non == "oui":
            choix = input("choisir obj")
            if choix == "potion_force" and self.inventory["potion_force"] > 0:
                self.strength += 10
                self.inventory["potion_force"] -= 1
                print(self.inventory)
                print("Vous avez maintenant {} de force".format(self.strength))
            elif choix == "potion_regen" and self.inventory["potion_regen"] > 0 and self.HP < 100:
                self.HP += 10
                self.inventory["potion_regen"] -= 1
                print(self.inventory)
                print("Vous avez maintenant {} de PV".format(self.HP))
            elif choix == "potion_defense" and self.inventory["potion_defense"] > 0:
                self.defense += 10
                self.inventory["potion_defense"] -= 1
                print(self.inventory)
                print("Vous avez maintenant {} de defense".format(self.defense))
            else:
                pass

    def buy_merchant(self):
        print("1: Achat d'une potion de force \n 2: Achat d'une potion de regen \n 3: Achat d'une potion de defense")
        choice = input()
        if choice == "1" and self.merchant_inventory["potion_force"] > 0:
            PlayerRPG.merchant_inventory["potion_force"] -= 1
            self.inventory["potion_force"] += 1
            print("Vous avez maintenant {} potion de force".format(self.inventory["potion_force"]))
        else:
            print("Error")

    def Event(self):
        position = self.perform_move()

        if position == "feu de camp":
            self.HP += 20
            print("vous avez désormais {}".format(self.HP))

        elif position == "coffre":
            list_objet = ["potion_defense", "potion_force", "potion_regène", "PO"]
            objet_win = random.choice(["potion de force", "potion de defense", "potion de soin", "PO"])
            if objet_win in self.inventory:
                self.inventory[objet_win] += 1
                print("bravo vous avez gagné {} votre inventaire est maintenant de {}".format(objet_win, self.inventory))
            else:
                self.inventory[objet_win] = 1
                print("bravo vous avez gagné {} votre inventaire est maintenant de {}".format(objet_win, self.inventory))

        elif position == "goblin":
            # combat(Player,goblin)
            print("goblin")

        elif position == "ogre":
            # combat(Player,ogre)
            print("ogre")

        elif position == "dragon":
            # combat(Player,dragon)
            print("dragon")

        elif position == "the final":
            # faire appel a la class monstre
            print("final")

        elif position == "piège - 50pv":
            self.HP -= 50
            print("dommage vous vous êtes pris un piège - 50pv vos pv sont donc de  {}".format(self.HP))

        elif position == "super potion":
            gain = random.choice(["potion_defense", "potion_force", "potion_regène"])
            self.inventory[gain] += 1
            print("super vous avez gangé une super {}".format(gain))

        elif position == "voleur de divers":
            cle_aleatoire = random.choice(list(self.inventory.keys()))
            if self.inventory[cle_aleatoire] > 0:
                self.inventory[cle_aleatoire] -= 1
                print("le voleur vous as pris {}".format(cle_aleatoire))
            else:
                self.HP -= 10
                print("vous ave222z rien par consequent tu perd 10hP")

        elif position == "piège":
            self.HP -= 10
            print("vous etes tomber sur un piège par conseqaunt vous perder 10pv il sont maintenant {} de Pv".format(self.HP))


player1 = PlayerRPG()
player1.map_init()

nb_deplacement = 0
while self.HP >=0:
    player1.inventory_use()
    player1.Event()
    nb_deplacement += 1
