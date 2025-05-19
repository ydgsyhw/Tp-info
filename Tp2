#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    char jour[3];
    char mois[3];
    char annee[5];
} Date;

typedef struct {
    char *nom;
    char reference[100];
    float prix;
    Date date_achat;
} Produit;

typedef struct Liste {
    Produit data;
    struct Liste *suivant;
} Liste;

Liste *creer_liste() {
    return NULL;
}

int est_vide(Liste *tete) {
    return (tete == NULL);
}

Liste *creer_produit() {
    Liste *nouveau = (Liste *)malloc(sizeof(Liste));
    if (!nouveau) {
        printf("Erreur d'allocation mémoire\n");
        return NULL;
    }

    nouveau->data.nom = (char *)malloc(100 * sizeof(char));
    if (!nouveau->data.nom) {
        printf("Erreur d'allocation mémoire pour le nom\n");
        free(nouveau);
        return NULL;
    }

    printf("Nom du produit : ");
    scanf(" %[^\n]", nouveau->data.nom);
    printf("Référence du produit : ");
    scanf(" %s", nouveau->data.reference);
    printf("Montant : ");
    scanf("%f", &nouveau->data.prix);
    printf("Date d'achat (JJ MM AAAA) : ");
    scanf("%s %s %s", nouveau->data.date_achat.jour,
                     nouveau->data.date_achat.mois,
                     nouveau->data.date_achat.annee);

    nouveau->suivant = NULL;
    return nouveau;
}

void afficher_produit(Liste *produit) {
    if (produit == NULL) {
        printf("Produit inexistant.\n");
        return;
    }
    printf("Nom: %s, Référence: %s, Montant: %.2f, Date Achat: %s/%s/%s\n",
           produit->data.nom,
           produit->data.reference,
           produit->data.prix,
           produit->data.date_achat.jour,
           produit->data.date_achat.mois,
           produit->data.date_achat.annee);
}

Liste *ajouter_debut(Liste *tete) {
    Liste *nouveau = creer_produit();
    if (nouveau) {
        nouveau->suivant = tete;
    }
    return nouveau;
}

void ajouter_fin(Liste **tete) {
    Liste *nouveau = creer_produit();
    if (!*tete) {
        *tete = nouveau;
    } else {
        Liste *temp = *tete;
        while (temp->suivant != NULL) {
            temp = temp->suivant;
        }
        temp->suivant = nouveau;
    }
}

void afficher_liste(Liste *tete) {
    if (est_vide(tete)) {
        printf("La liste est vide.\n");
        return;
    }

    Liste *temp = tete;
    while (temp != NULL) {
        afficher_produit(temp);
        temp = temp->suivant;
    }
}

int longueur_liste(Liste *tete) {
    int compteur = 0;
    while (tete != NULL) {
        compteur++;
        tete = tete->suivant;
    }
    return compteur;
}

void filtrer_par_date(Liste *tete, Date date) {
    Liste *temp = tete;
    while (temp != NULL) {
        if (strcmp(temp->data.date_achat.jour, date.jour) == 0 &&
            strcmp(temp->data.date_achat.mois, date.mois) == 0 &&
            strcmp(temp->data.date_achat.annee, date.annee) == 0) {
            afficher_produit(temp);
        }
        temp = temp->suivant;
    }
}

Liste *supprimer_debut(Liste *tete) {
    if (est_vide(tete)) return NULL;

    Liste *temp = tete;
    tete = tete->suivant;
    free(temp->data.nom);
    free(temp);
    return tete;
}

Liste *supprimer_fin(Liste *tete) {
    if (est_vide(tete)) return NULL;

    if (tete->suivant == NULL) {
        free(tete->data.nom);
        free(tete);
        return NULL;
    }

    Liste *temp = tete;
    Liste *precedent = NULL;

    while (temp->suivant != NULL) {
        precedent = temp;
        temp = temp->suivant;
    }

    precedent->suivant = NULL;
    free(temp->data.nom);
    free(temp);
    return tete;
}

Liste *supprimer_par_reference(Liste *tete, char *ref) {
    if (est_vide(tete)) return NULL;

    if (strcmp(tete->data.reference, ref) == 0) {
        return supprimer_debut(tete);
    }

    Liste *temp = tete;
    Liste *precedent = NULL;

    while (temp != NULL && strcmp(temp->data.reference, ref) != 0) {
        precedent = temp;
        temp = temp->suivant;
    }

    if (temp == NULL) {
        printf("Produit non trouvé.\n");
        return tete;
    }

    precedent->suivant = temp->suivant;
    free(temp->data.nom);
    free(temp);
    return tete;
}

int main() {
    Liste *stock = creer_liste();
    int choix;
    char ref[100];
    Date date;

    do {
        printf("\nMenu :\n");
        printf("1. Ajouter un produit au début\n");
        printf("2. Ajouter un produit à la fin\n");
        printf("3. Afficher tous les produits\n");
        printf("4. Supprimer le premier produit\n");
        printf("5. Supprimer le dernier produit\n");
        printf("6. Supprimer un produit par référence\n");
        printf("7. Afficher les produits achetés à une date\n");
        printf("8. Quitter\n");
        printf("Choix : ");
        scanf("%d", &choix);

        switch (choix) {
            case 1:
                stock = ajouter_debut(stock);
                break;
            case 2:
                ajouter_fin(&stock);
                break;
            case 3:
                afficher_liste(stock);
                break;
            case 4:
                stock = supprimer_debut(stock);
                break;
            case 5:
                stock = supprimer_fin(stock);
                break;
            case 6:
                printf("Entrez la référence : ");
                scanf("%s", ref);
                stock = supprimer_par_reference(stock, ref);
                break;
            case 7:
                printf("Entrez la date (JJ MM AAAA) : ");
                scanf("%s %s %s", date.jour, date.mois, date.annee);
                filtrer_par_date(stock, date);
                break;
            case 8:
                printf("Programme terminé.\n");
                break;
            default:
                printf("Choix invalide.\n");
        }
    } while (choix != 8);

    return 0;
}
