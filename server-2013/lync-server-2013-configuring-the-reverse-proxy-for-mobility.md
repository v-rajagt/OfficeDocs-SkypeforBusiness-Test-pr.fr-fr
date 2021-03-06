﻿---
title: 'Lync Server 2013 : Configuration du proxy inverse pour la mobilité'
TOCTitle: Configuration du proxy inverse pour la mobilité
ms:assetid: 3f4a9e33-77e4-4c18-a73f-24d4bec8ea9c
ms:mtpsurl: https://technet.microsoft.com/fr-fr/library/Hh690011(v=OCS.15)
ms:contentKeyID: 49296995
ms.date: 05/20/2016
mtps_version: v=OCS.15
ms.translationtype: HT
---

# Configuration du proxy inverse pour la mobilité dans Lync Server 2013

 

_**Dernière rubrique modifiée :** 2014-03-20_

Si vous souhaitez utiliser la découverte automatique pour des clients d’appareils mobiles, vous devez modifier une règle de publication web existante ou en créer une pour le proxy inverse, que vous mettiez ou non à jour les listes d’autres noms de sujet sur les certificats de proxy inverse.

Si vous décidez d’utiliser HTTPS pour les demandes initiales du service de découverte automatique de Lync Server 2013 et de mettre à jour les listes d’autres noms de sujet sur les certificats de proxy inverse, vous devez affecter le certificat public mis à jour à l’écouteur SSL (Secure Sockets Layer) sur votre proxy inverse. Pour plus d’informations sur les entrées d’autres noms de sujet requises, reportez-vous à [Exigences techniques pour la mobilité dans Lync Server 2013](lync-server-2013-technical-requirements-for-mobility.md). Vous devez ensuite créer modifier l’écouteur existant pour les services web externes ou créer une règle de publication web pour l’URL du service de découverte automatique externe. Si vous n’avez pas encore de règle de publication web pour l’URL externe des services web Lync Server 2013 de votre pool de serveurs frontaux, vous devez également publier une règle pour cette URL.

<table>
<thead>
<tr class="header">
<th><img src="images/Gg398920.note(OCS.15).gif" title="note" alt="note" />Remarque :</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>La règle de publication et l’écouteur du proxy inverse peuvent être utilisés par les services web externes et le service de découverte automatique, à condition que le certificat affecté à l’écouteur contienne le nom de sujet et les autres noms de sujet nécessaires aux deux. Pour plus d’informations sur la configuration par défaut de l’écouteur web et de la règle de publication, reportez-vous à <a href="lync-server-2013-setting-up-reverse-proxy-servers.md">Configuration des serveurs proxy inverses pour Lync Server 2013</a>.</td>
</tr>
</tbody>
</table>


Si vous décidez d’utiliser HTTP pour les demandes initiales du service de découverte automatique pour ne pas avoir à mettre à jour les autres noms de sujet pour le proxy inverse, vous devez créer ou modifier une règle de publication web pour le port 80.

Les procédures de cette section indiquent comment créer ou modifier les règles de publication web dans Microsoft Forefront Threat Management Gateway 2010 pour la découverte automatique.

<table>
<thead>
<tr class="header">
<th><img src="images/Gg398920.note(OCS.15).gif" title="note" alt="note" />Remarque :</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Ces procédures supposent que vous avez installé la version Standard Edition de Forefront Threat Management Gateway (TMG) 2010. Si vous utilisez un autre proxy inverse, les procédures, bien qu’identiques, devront être adaptées en tenant compte de la documentation du produit tiers.</td>
</tr>
</tbody>
</table>


## Pour créer une règle de publication web pour l’URL de découverte automatique externe

1.  Cliquez sur **Démarrer** , pointez sur **Programmes** , sur **Microsoft Forefront TMG** , puis cliquez sur **Forefront TMG Management** .

2.  Dans le volet de gauche, développez **NomServeur** , cliquez avec le bouton droit sur **Stratégie de pare-feu** , pointez sur **Nouveau** , puis cliquez sur **Règle de publication web** .

3.  Dans la page **Assistant Nouvelle règle de publication web** , entrez un nom convivial pour la nouvelle règle de publication (par exemple, LyncDiscoveryURL).

4.  Dans la page **Sélectionner l’action de la règle** , sélectionnez **Autoriser** .

5.  Dans la page **Type de publication** , sélectionnez **Publier un seul site web ou équilibreur de charge** .

6.  Dans la page **Sécurité de connexion serveur** , sélectionnez **Utiliser SSL pour se connecter au serveur web publié ou à la batterie de serveurs** .

7.  Dans la page **Détails de publication internes** , dans **Nom du site interne** , tapez le nom de domaine complet (FQDN) de votre pool de directeurs (par exemple, lyncdir01.contoso.local). Si vous créez une règle pour l’URL des services web dans pool de serveurs frontaux, tapez l’adresse VIP de l’équilibreur de la charge matérielle (HLB) devant le pool de serveurs frontaux.

8.  Dans la page **Détails de publication internes** , dans **Chemin d’accès (facultatif)** , tapez **/\*** comme chemin d’accès du dossier à publier, puis sélectionnez **Transférer l’en-tête de l’hôte d’origine** .

9.  Dans la page **Informations sur les noms publics** , procédez comme suit :
    
      - Sous **Accepter les demandes pour** , sélectionnez **Ce nom de domaine** .
    
      - Dans **Nom public** , tapez **lyncdiscover.** *\<domaineSIP\>* (URL du service de découverte automatique externe). Si vous créez une règle pour l’URL des services web externes sur le pool de serveurs frontaux, tapez le nom de domaine complet des services web externes sur votre pool de serveurs frontaux (par exemple, lyncwebextpool01.contoso.com).
    
      - Dans **Chemin d’accès** , tapez **/\*** .

10. Dans la page **Sélectionner le port d’écoute** , dans **Port d’écoute web** , sélectionnez votre écouteur SSL existant avec le certificat public mis à jour.

11. Dans la page **Délégation de l’authentification** , sélectionnez **Aucune délégation pour laisser le client s’authentifier directement** .

12. Dans la page **Ensemble d’utilisateurs** , cliquez sur **Tous les utilisateurs** .

13. Dans la page **Fin de l’Assistant Nouvelle règle de publication web** , vérifiez que les paramètres de la règle de publication web sont corrects, puis cliquez sur **Terminer** .

14. Dans la liste Forefront TMG de règles de publication web, double-cliquez sur la nouvelle règle que vous venez d’ajouter pour ouvrir les **Propriétés** .

15. Sous l’onglet **À** , procédez comme suit :
    
      - Sélectionnez **Transmettre l’en-tête de l’hôte d’origine plutôt que l’en-tête réel** .
    
      - Sélectionnez **Les demandes semblent émaner de l’ordinateur Forefront TMG** .

16. Sous l’onglet **Pontage** , configurez ce qui suit :
    
      - Sélectionnez **Serveur web** .
    
      - Sélectionnez **Rediriger les demandes au port HTTP** et tapez **8080** comme numéro de port.
    
      - Sélectionnez **Rediriger les demandes au port SSL** et tapez **4443** comme numéro de port.

17. Cliquez sur **OK** .

18. Cliquez sur **Appliquer** dans le volet des détails pour enregistrer les modifications et mettre à jour la configuration.

19. Cliquez sur **Tester la règle** pour vérifier que votre nouvelle règle est configurée correctement.

## Pour modifier une règle de publication web existante afin d’ajouter l’autre nom de sujet et l’URL du service de découverte automatique externe

1.  Cliquez sur **Démarrer** , pointez sur **Programmes** , sur **Microsoft Forefront TMG** , puis cliquez sur **Forefront TMG Management** .
    
    <table>
    <thead>
    <tr class="header">
    <th><img src="images/Gg425917.important(OCS.15).gif" title="important" alt="important" />Important :</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Vous devrez répéter la modification pour chaque règle de publication et chaque écouteur que vous possédez. En règle générale, il existe une règle et un écouteur pour les pools de serveurs frontaux et une règle et un écouteur pour les directeurs ou les directeur facultatifs (si vous les avez déployés).</td>
    </tr>
    </tbody>
    </table>


2.  Dans le volet gauche, développez **NomServeur** , cliquez avec le bouton droit sur **Stratégie de pare-feu** , puis cliquez sur la règle applicable. Sous l’onglet **Tâches** , cliquez sur **Modifier la règle sélectionnée** .

3.  Sous l’onglet **Nom public** , dans **Cette règle s’applique à** , sélectionnez **Demandes pour les sites web suivants** .

4.  Cliquez sur **Ajouter** , tapez le nom du nouveau site de découverte automatique (par exemple, « lyncdiscover.contoso.com »), puis cliquez sur **OK** .

5.  Sous l’onglet **Écouteur** , cliquez sur **Sélectionner un certificat** et affectez le nouveau certificat avec les entrées d’autres noms de sujet ajoutées du service de découverte automatique. Fermez les propriétés d’écouteur et de publication web.

6.  Cliquez sur **Appliquer** dans le volet des détails pour enregistrer les modifications et mettre à jour la configuration.

7.  Cliquez sur **Tester la règle** pour vérifier que votre nouvelle règle est configurée correctement.

## Pour créer une règle de publication web pour le port 80

1.  Cliquez sur **Démarrer** , pointez sur **Programmes** , sur **Microsoft Forefront TMG** , puis cliquez sur **Forefront TMG Management** .

2.  Dans le volet de gauche, développez **NomServeur** , cliquez avec le bouton droit sur **Stratégie de pare-feu** , pointez sur **Nouveau** , puis cliquez sur **Règle de publication web** .

3.  Dans la page **Assistant Nouvelle règle de publication web** , entrez un nom convivial pour la nouvelle règle de publication (par exemple, Lync Autodiscover \[HTTP\]).

4.  Dans la page **Sélectionner l’action de la règle** , sélectionnez **Autoriser** .

5.  Dans la page **Type de publication** , sélectionnez **Publier un seul site web ou équilibreur de charge** .

6.  Dans la page **Sécurité de connexion serveur** , sélectionnez **Utiliser des connexions non sécurisées pour se connecter au serveur web publié ou à la batterie de serveurs** .

7.  Dans la page **Détails de publication internes** , dans **Nom du site interne** , tapez l’adresse VIP de l’équilibreur de la charge matérielle (HLB) devant le pool de serveurs frontaux.

8.  Dans la page **Détails de publication internes** , dans **Chemin d’accès (facultatif)** , tapez **/\*** comme chemin d’accès du dossier à publier, puis sélectionnez **Transférer l’en-tête de l’hôte d’origine au lieu de celui spécifié dans le champ Nom du site interne** .

9.  Dans la page **Informations sur les noms publics** , procédez comme suit :
    
      - Sous **Accepter les demandes pour** , sélectionnez **Ce nom de domaine** .
    
      - Dans **Nom public** , tapez **lyncdiscover.** *\<domaineSIP\>* (URL du service de découverte automatique externe).
    
      - Dans **Chemin d’accès** , tapez **/\*** .

10. Dans la page **Sélectionner un port d’écoute** , dans **Port d’écoute web** , sélectionnez un écouteur web ou utilisez l’Assistant Nouvelle définition d’écouteur web pour en créer un nouveau.

11. Dans la page **Délégation de l’authentification** , sélectionnez **Aucune délégation et le client ne peut pas authentifier directement** .

12. Dans la page **Ensemble d’utilisateurs** , cliquez sur **Tous les utilisateurs** .

13. Dans la page **Fin de l’Assistant Nouvelle règle de publication web** , vérifiez que les paramètres de la règle de publication web sont corrects, puis cliquez sur **Terminer** .

14. Dans la liste Forefront TMG de règles de publication web, double-cliquez sur la nouvelle règle que vous venez d’ajouter pour ouvrir les **Propriétés** .

15. Sous l’onglet **Pontage** , configurez ce qui suit :
    
      - Sélectionnez **Serveur web** .
    
      - Sélectionnez **Rediriger les demandes au port HTTP** et tapez **8080** comme numéro de port.
    
      - Vérifiez que l’option **Rediriger les demandes au port SSL** n’est pas sélectionnée.

16. Cliquez sur **OK** .

17. Cliquez sur **Appliquer** dans le volet des détails pour enregistrer les modifications et mettre à jour la configuration.

18. Cliquez sur **Tester la règle** pour vérifier que votre nouvelle règle est configurée correctement.

19. Vérifiez que l’URL du service de découverte automatique externe n’est définie sur aucune autre règle de publication web.

## Voir aussi

#### Concepts

[Configuration des serveurs proxy inverses pour Lync Server 2013](lync-server-2013-setting-up-reverse-proxy-servers.md)  
[Exigences techniques pour la mobilité dans Lync Server 2013](lync-server-2013-technical-requirements-for-mobility.md)

