# Android-Rooting

-->Définition rooting (4 phrases):
Le rooting est le processus par lequel un utilisateur obtient les privileges superutilisateur sur un appareil Android. un environnement root permet d'observer des comportements systeme normalement inaccessibles, d'analyser le stockage des donnees sensibles et de tester la robustesse des controles de securite.

-->Schéma simple Verified Boot / AVB (chaîne de confiance):
Verified Boot garantit que le systeme qui demarre est exactement celui prevu par le fabricant, sans aucune modification malveillante. Il permet de controler l'integrite au demarrage.

    -->flux de verified boot:
rom(read-only memory)-->bootloader(verifie signatur de la partition boot)-->kernel android(verifie les partitions systeme-->partitions-->applicaions

AVB est l'evolution moderne de Verified Boot : il ajoute une verification cryptographique de l'integrite de chaque partition via des descripteurs dans les metadonnees vbmeta. Il integre une protection anti-rollback qui empeche d'installer d'anciennes versions du systeme contenant des failles connues.


-->RISQUE
Integrite non garantie — conclusions biaisees sur la securite reelle de l'application.
Surface d'attaque accrue si l'appareil quitte le labo — exposition a des menaces externes.
Donnees sensibles exposees si presentes — violation potentielle de confidentialite.
Instabilite systeme — tests non reproductibles et resultats incoherents.
Melange comptes perso/test — fuite possible d'informations personnelles.
Mauvais nettoyage en fin de seance — persistance de donnees sensibles.
Reseau non isole — effets involontaires sur systemes externes.
Tracabilite insuffisante — impossible de reproduire ou d'auditer les tests.

-->MESURE DEFENSIVE
Documenter l'etat du systeme (verifiedbootstate) avant chaque test.
Device/AVD dedie exclusivement aux tests, jamais sorti du perimetre.
Utiliser uniquement des donnees fictives generees pour les tests.
Snapshots AVD avant chaque session ; reproductibilite verifiee.
Aucun compte personnel — AVD vierge sans Gmail, Google Play.
Wipe AVD systematique en fin de seance + preuve capture.
Reseau de test dedié — aucune connexion a des services de production.
Journal horodate + captures de toutes les etapes cles.

-->OWASP MASVS — 2 Exigences Resumees
STORAGE-1 : Stockage securise des donnees sensibles
Les donnees sensibles (cles API, tokens d'authentification, mots de passe) doivent etre stockees via des mecanismes de chiffrement adaptes (Android Keystore), jamais en clair dans les SharedPreferences, fichiers texte ou bases SQLite non chiffrees.

NETWORK-1 : Communications reseau securisees
Toutes les communications doivent utiliser TLS avec une configuration correcte et la verification stricte des certificats. L'application ne doit pas accepter de certificats auto-signes ou invalides en production.

-->OWASP MASTG — 2 Idees de Tests

Inspection du stockage interne avec privileges root
Analyse des logs applicatifs pour fuites d'informations

