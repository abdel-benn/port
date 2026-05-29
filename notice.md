# Notice TP – VoIP / SIP (Guide de réalisation)

## 1. Lancer l’environnement

1. Brancher le PC sur le réseau du TP.
2. Vérifier que le PC reçoit une IP en `10.127.1.200 → 10.127.1.252`.
3. Ouvrir le serveur SIP :
   - http://10.127.1.6:18080/sip/gate
   - login : `admin`
   - mdp : `Adminetu1`

---

## 2. Créer les comptes SIP

1. Se connecter au serveur SIP.
2. Aller dans **gestion des utilisateurs**.
3. Créer 4 comptes :
   - User ID : 1000 à 1003
   - Password : identique au user
4. Valider chaque création.

---

## 3. Configurer MicroSIP

1. Lancer **Wireshark** et démarrer la capture réseau.
2. Ouvrir **MicroSIP**.
3. Ajouter un compte SIP :
   - User : 1000
   - Password : 1000
   - Domain : 10.127.1.6
4. Valider.
5. Vérifier que le statut passe à **Online**.
6. Vérifier sur le serveur que le client est enregistré.

---

## 4. Observer l’enregistrement SIP

1. Laisser MicroSIP connecté.
2. Dans Wireshark, filtrer :
   - `sip`
3. Identifier les messages :
   - REGISTER
   - 401 Unauthorized
   - REGISTER (auth)
   - 200 OK
4. Repérer le champ :
   - `Expires`

---

## 5. Désenregistrer un client

1. Fermer MicroSIP OU désactiver le compte.
2. Observer dans Wireshark :
   - REGISTER avec `Expires: 0`

---

## 6. Tester les erreurs

1. Mauvais user :
   - changer le User ID
   - observer `404` ou `403`
2. Mauvais mot de passe :
   - garder user correct
   - observer `401 Unauthorized`

---

## 7. Réaliser un appel SIP

1. Configurer 2 comptes (ex : 1000 et 1001).
2. Démarrer Wireshark.
3. Depuis 1000, appeler 1001.
4. Observer dans Wireshark :
   - INVITE
   - 100 Trying
   - 180 Ringing
   - 200 OK
   - ACK
   - BYE

---

## 8. Tester les cas d’appel

1. Appel occupé :
   - vérifier réponse `486 Busy Here`
2. Téléphone débranché :
   - lancer appel vers client enregistré mais offline
   - observer échec

---

## 9. Tester présence

1. Activer fonction présence dans MicroSIP (si dispo).
2. Observer dans Wireshark :
   - SUBSCRIBE
   - NOTIFY

---

## 10. Tester chat SIP

1. Envoyer un message texte via MicroSIP.
2. Observer dans Wireshark :
   - MESSAGE
   - 200 OK

---

## 11. Configurer téléphone IP Yealink

1. Brancher le téléphone sur le switch.
2. Récupérer son adresse IP (menu téléphone).
3. Accéder via navigateur :
   - http://IP_du_téléphone
4. Login :
   - admin / admin
5. Configurer le compte SIP :
   - user / password / serveur
6. Tester appel.

---

## 12. Tester RTP / codec

1. Lancer un appel.
2. Dans Wireshark, filtrer :
   - `rtp`
3. Vérifier flux audio.
4. Changer codec (ex : G.722) dans config si demandé.
5. Relancer appel.

---

## 13. Tester DTMF

1. Pendant un appel, taper des chiffres.
2. Observer dans Wireshark :
   - RTP Event ou SIP INFO ou Inband
3. Vérifier la méthode utilisée.

---

## 14. Tester UDP / TCP / TLS

1. Modifier transport SIP (si possible).
2. Comparer dans Wireshark :
   - UDP
   - TCP
   - TLS (chiffré)

---

## 15. Tester SRTP

1. Activer SRTP dans configuration SIP.
2. Lancer un appel.
3. Vérifier :
   - flux RTP illisible dans Wireshark

---

## 16. Tester echo / qualité

1. Activer haut-parleur.
2. Tester appel.
3. Activer/désactiver annulation d’écho.
4. Observer différence audio.

---

## 17. Tester VAD / CNG

1. Lancer appel.
2. Ne pas parler.
3. Observer :
   - arrêt des paquets (VAD)
   - bruit de fond (CNG)

---

## 18. Auto-provisioning

1. Brancher téléphone IP.
2. Vérifier DHCP actif.
3. Vérifier récupération config automatique.
4. Vérifier compte SIP déjà configuré.

---

## Conclusion

Le TP consiste à :
- configurer SIP
- tester appels et messages
- analyser Wireshark
- configurer téléphone IP
- tester sécurité et codecs