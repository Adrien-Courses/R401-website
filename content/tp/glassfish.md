+++
title = "Glassfish"
weight = 10
+++

## Trop chiant
GlassFish7 n'a plus de Pluguin pour Eclipse (non maintenu). Pour faire ce qu'on veut MVC2 on peut utiliser uniquement Tomcat qui est juste pour le WEB (voir Tomcat ws GlassFish) et ceci sera emplement suffisant

![webprogile](../image/webprofile.png)



## Glassfish
Glassfish est un projet Eclipse et télécharger [Eclipse GlassFish 7.0.18, Jakarta EE Platform, 10](https://www.eclipse.org/downloads/download.php?file=/ee4j/glassfish/glassfish-7.0.18.zip). C'est un zip que vous mettez ou vous voulez.

Pour lancer Glassfish rendez-vous dans ~/Developer/java-util/glassfish7/glassfish/bin (ATTENTION pas ~/Developer/java-util/glassfish7/bin) puis

`~/Developer/java-util/glassfish7/glassfish/bin : ./asadmin start-domain`

Si vous avez une NPE ceci peut être du au fait que vous n'êtes pas de le répertoire ou que vous n'avez pas la bonne version de java

## Panel Admin
On peut maintenant accéder au panel administratif de Glassfish http://localhost:4848/common/index.jsf


## Connecter Eclipse à Glassfish
https://omnifish.ee/developers/glassfish-server/ide-plugins-for-glassfish/eclipse-ide/

Avant de commencer quoi que ce soit, nous devons connecter Glassfish à Eclipse
- OmniFish tools for Eclipse GlassFish – works with GlassFish 7. It’s actively maintained by OmniFish. it’s based on the older GlassFish Tools plugin and it works very similarly to it.
- GlassFish Tools plugin – works with GlassFish 5, 4, and 3.1. Doesn’t work with GlassFish 7. It’s not actively maintained anymore.

Nous GlassFish7 => OmniFish


![alt text](../images/eclipse_glassfish.png)
