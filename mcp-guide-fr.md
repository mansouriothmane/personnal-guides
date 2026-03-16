# Guide essentiel MCP & IA agentique

## 1. Notions de base
- **LLM** : modèle de langage qui génère du texte.  
- **Agent** : LLM qui peut planifier, utiliser des outils et analyser les résultats.  
- **MCP** : protocole qui permet à un agent d’appeler des outils de manière standardisée.

---

## 2. Termes techniques courants
- **Token** : unité de texte. Plus il y en a, plus c’est lent et coûteux.  
- **Contexte** : quantité de texte que le modèle peut lire en même temps.  
- **Inference** : génération de la réponse.  
- **Latence** : temps de réponse.  
- **Throughput** : nombre de requêtes traitées par seconde.  
- **Hallucination** : réponse incorrecte inventée.

---

## 3. Cycle de vie d’un agent
1. Reçoit un objectif  
2. Analyse  
3. Choisit un outil  
4. Exécute  
5. Observe  
6. Continue ou conclut  

---

## 4. MCP en bref
- Décrit les outils via des **schemas JSON**.  
- Permet la **découverte** et l’**appel** d’outils.  
- Gère les **permissions** et la **sécurité**.  
- Structure les échanges entre agent et services.

---

## 5. Notions souvent mentionnées
- **RAG** : récupération de documents avant réponse.  
- **Embeddings** : représentation numérique de textes.  
- **Guardrails** : règles de sécurité.  
- **Prompt engineering** : formulation des instructions.

---

## 6. Formules simples
- **Accuracy** = bonnes réponses / total  
- **Coût** = tokens × prix  
- **Latence totale** = modèle + outils + réseau  

---

## 7. Architecture typique
- LLM  
- Serveur MCP  
- Outils (API, services internes)  
- Vector store  
- Orchestration (framework agentique)

---

## 8. Questions utiles
- Comment les outils sont-ils décrits dans le MCP ?  
- Quelles optimisations réduisent les tokens ?  
- Comment évaluez-vous la qualité des actions de l’agent ?  
- Quels mécanismes de sécurité encadrent les outils ?  
