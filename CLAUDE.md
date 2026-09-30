# Step3D — ce qu'il faut savoir

Outil d'annotation de la marche : on choisit un sujet, une visite, un essai ; on regarde
les marqueurs en 3D et on place les instants de pose (pose du pied) et de lever.
Une seule page : `index.html`. Public : étudiants sage-femme / analyse de marche.

## Base de données Supabase

Projet : `obclhzckhzrzhwcucmns` (l'URL et la clé publishable sont dans `index.html`).
Toutes les tables ont RLS activé avec accès public (le site n'a pas de connexion).

- `sujet` : `id_sujet`, `code` (S01, S02…)
- `visite` : `id_visite`, `id_sujet`, `numero`, `date_visite`
- `essai` : `id_essai`, `id_visite`, `numero`, `fichier` (nom du JSON), `frequence_hz`, `nb_images`
- `evenement` : `id_evenement`, `id_essai`, `pied` (gauche/droit), `type` (pose/lever),
  `image`, `temps_s`

Conventions :
- le numéro d'image démarre à 0 ; `temps_s = image / frequence_hz` ;
- un essai est « annoté » dès qu'il a au moins un événement en base ;
- l'enregistrement remplace tous les événements de l'essai (effacement + réinsertion).

## Les essais (fichiers JSON)

Dans `essais/`, servis par le site et chargés par `fetch('essais/<fichier>')`.
Contenu : `sujet`, `visite`, `essai`, `frequence_hz` (100), `unite` (m), `axe_vertical`
(z = hauteur), `nb_images`, `marqueurs` (20 marqueurs nommés L_/R_ : tableaux [x, y, z],
une entrée par image) et `contact_sol` ({gauche, droit} : 0/1 par image, pied au sol ou non).

- Un marqueur (0, 0, 0) n'est pas capté : ne pas l'afficher.
- `contact_sol` sert de guide visuel (pistes sous la timeline) et de pré-remplissage.

## Raccourcis clavier

Q / D : image précédente / suivante ; Z / S : ±10 images ; Espace : lecture / pause ;
W : pose puis lever du pied gauche ; B : idem pied droit ; Ctrl+Z : annuler.