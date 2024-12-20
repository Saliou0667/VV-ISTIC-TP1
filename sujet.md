# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### 1. Bug FaceTime chez Apple (janvier 2019)
Un exemple connu est le bug FaceTime découvert chez Apple en janvier 2019. Ce bug permettait à un utilisateur d’entendre le son (voire de voir la vidéo, dans certains cas) de l’interlocuteur avant même que celui-ci n’accepte l’appel, simplement en utilisant la fonctionnalité d’appel de groupe sur FaceTime. Ce problème peut être considéré comme un bug local, ciblé sur une fonctionnalité spécifique (les appels de groupe FaceTime), mais ses conséquences étaient globales en termes d’impact sur la vie privée.

La « failure » (la manifestation du défaut) se produisait lorsque l’appelant ajoutait son propre numéro de téléphone au sein d’un appel de groupe FaceTime avant que le destinataire n’ait répondu. Le système, mal géré, activait alors le micro (et potentiellement la caméra) du destinataire sans son consentement.

Les répercussions pour les utilisateurs ont été sérieuses en termes de confidentialité et de respect de la vie privée. Pour Apple, cela a entraîné une mauvaise presse, une perte de confiance de certains utilisateurs, ainsi que la nécessité de désactiver temporairement la fonction d’appel de groupe, puis de publier une mise à jour rapide pour corriger le bug.

Si des scénarios de test plus complets, y compris la tentative d’ajouter un appelant dans des conditions atypiques (tels que l’appelant ajoutant lui-même son numéro), avaient été envisagés, le bug aurait probablement pu être découvert avant la mise en production. Un test ciblé sur la fonctionnalité d’appel en groupe et les interactions inhabituelles entre l’appelant et le destinataire aurait pu révéler la faille.

---

### 2. Apache Commons Bug Analysis
Parmi les problèmes résolus dans Apache Commons Collections, on peut considérer un exemple hypothétique basé sur un décorateur de collection mal implémenté, tel que l’issue COLLECTIONS-684 : « BoundedCollection decorator does not fulfill general contract of Collection ». Ce bug était local, car il concernait spécifiquement le comportement d’un décorateur particulier (BoundedCollection) au sein de la bibliothèque, et non l’ensemble des collections ou la totalité de la bibliothèque.

Le bug provenait d’un non-respect des conditions générales du contrat de l’interface Collection, ce qui pouvait engendrer des comportements inattendus lors de certaines opérations (par exemple, un `add()` qui échoue silencieusement ou ne se comporte pas comme prévu). La solution a consisté à corriger la logique interne du décorateur afin qu’il respecte scrupuleusement le contrat de la Collection, par exemple en assurant que l’exception appropriée soit levée lorsque la limite est atteinte.

Les contributeurs du projet ont effectivement ajouté ou mis à jour des tests unitaires pour vérifier le comportement du `BoundedCollection` après la correction. Ces tests garantissent que si ce type de bug réapparaît à l’avenir, il sera détecté rapidement.

---

### 3. Netflix Chaos Engineering
Netflix pratique le Chaos Engineering, consistant à injecter intentionnellement des pannes dans leurs systèmes de production. Par exemple, ils arrêtent volontairement certains serveurs ou services (via des outils comme Chaos Monkey) afin d’observer la résilience de l’infrastructure.

**Exigences pour ces expériences :**
- Une architecture distribuée et résiliente.
- Des mécanismes de surveillance avancés (logs, métriques temps réel).
- Des plans de rétablissement rapide.
- Des protocoles de sécurité et de limitation des effets négatifs sur les utilisateurs.

**Variables observées :**
- La disponibilité du service (peut-on toujours diffuser du contenu ?).
- Le temps de réponse des services restants.
- Le niveau d’erreurs (taux d’échec des requêtes, dégradations de la qualité).
- L’impact sur l’expérience utilisateur.

Les résultats obtenus montrent que, grâce à ces tests en conditions réelles, Netflix parvient à augmenter considérablement la résilience, la robustesse et la capacité à tolérer des pannes imprévues. Cela leur permet d’identifier les points faibles et d’améliorer continuellement leur architecture.

Netflix n’est pas la seule entreprise à mener ce type d’expériences. D’autres grandes sociétés (comme Google, Amazon ou LinkedIn) utilisent également des formes de chaos testing ou des techniques similaires pour améliorer leur résilience. Dans d’autres organisations, il serait possible de simuler, par exemple :
- L’indisponibilité d’une base de données.
- L’augmentation anormale de la latence réseau.
- La perte de messages dans un système de messagerie.
- L’indisponibilité de microservices.

**Variables à observer :**
- Les taux d’erreur.
- Le comportement des mécanismes de repli.
- Les performances globales.
- Le temps de récupération du système.

---

### 4. WebAssembly Formal Specification
La spécification formelle de WebAssembly présente plusieurs avantages :
- Elle élimine un grand nombre d’ambiguïtés, garantissant que différentes implémentations suivent toutes exactement les mêmes règles.
- Elle facilite la vérification de propriétés formelles (sécurité, sûreté, terminaison) et permet de prouver mathématiquement la correction de certaines parties du langage.
- Elle fournit une référence solide et invariable pour les développeurs d’outils, de compilateurs et de moteurs d’exécution, minimisant les divergences d’interprétation.

Cependant, le fait de disposer d’une spécification formelle ne signifie pas que les implémentations de WebAssembly ne doivent pas être testées. Les tests restent essentiels pour détecter les problèmes pratiques, les erreurs d’intégration et les cas non prévus par la spécification (erreurs d’implémentation, aspects performance, intégration dans différents environnements, etc.). La spécification formelle réduit les risques de divergence conceptuelle, mais les tests demeurent un complément indispensable pour assurer la qualité et la robustesse des implémentations.

---

### 5. Mechanized Specification of WebAssembly
Les avantages principaux d’une spécification mécanisée (formalisée dans un assistant de preuve comme Isabelle) résident dans la possibilité d’effectuer des preuves formelles automatisées. Cela permet :
- De détecter des incohérences, erreurs ou ambiguïtés non vues dans la spécification initiale.
- D’assurer une plus grande confiance dans la correction et la cohérence globale des règles du langage.
- De faciliter l’évolution de la spécification en conservant la capacité à vérifier automatiquement la cohérence des modifications.

Cette mécanisation a effectivement contribué à améliorer la spécification formelle originale de WebAssembly, en mettant en évidence des points ambigus ou imprécis, ce qui a permis d’affiner et de corriger la spécification initiale.

En outre, la mécanisation a permis de dériver des artefacts tels que :
- Des preuves formelles de propriétés du langage.
- Des modèles exécutables dans l’assistant de preuve.
- Des outils d’analyse ou de vérification qui s’appuient directement sur cette spécification formelle mécanisée.

L’auteur a vérifié la spécification à l’aide d’Isabelle/HOL, en formalisant la sémantique du langage et en prouvant divers invariants et propriétés. Cependant, même avec une spécification mécanisée, le besoin de tests ne disparaît pas. Les tests sont complémentaires : ils détectent des erreurs d’implémentation, des problèmes de performance, des conditions limites non envisagées, ou encore des divergences entre l’implémentation et la spécification qui ne sont pas nécessairement couvertes par les preuves formelles. Une spécification mécanisée augmente la confiance dans la correction conceptuelle, mais n’annule pas l’intérêt des tests pratiques.
