# Config OsTicket

Le but du jeux c'est de notifier l'arriver d'un mail à un groupe de personnes celon l'adresse mail contacter.


Pour ce faire il faut distingué 4 endroit dans OsTicket partie admin
 * Emails -> Emails (S'occupe de la config smtp, imap, et le liens entre département et topic d'aide)
 * Manage -> Helps Topics (S'occupe d'assigné un ticket au bon département et la bonne team)
 * Agents -> Teams (S'occupe de notifier par email l'arriver d'un ticket)
 * Agents -> Departments (S'occupe de donnée les droits sur la vision des ticket celon l'email d'arrivé)

Avec cette configuration les emails qui arrive sur une de nos adresse est convertit en un ticket est assigné au bon département et à la bonne team. Ensuite OsTicket notifie les membres de la team de l'arrivé de ce mail.

Ce qui permet de voire facilement quel hub s'occupe d'un mail (vue que le département n'est pas indiqué dans la liste ou on voie les ticket).

Cela permet aussi d'ajouté des membre sur le support du ticket sans qu'il voie tout les ticket et aussi cela permet d'ajouté des nouveaux qu'ils veillent dans un premier temps juste voire comment cela marche sans les spams de mail avec les nouveau ticket.