
---

# **Git Cadence Guide**  
### *Overall strategy for developing features -- branch/merge strategy for ongoing development.*

---

## **Overview**
This guide explains:

- How to develop on feature branches and merge back on to the main branch.   

---

## **Ruleset**

Generally want to develop specific features on branches labeled `feature/<insert-feature-name>` or `bugfix/<insert-feature-description>` for specific bugixes to pre-existing features. Once development finishes on the corresponding branch, initiate merge and complete merge with Copilot or self approval. If features compound, establish child and parent branches by branching off of another feature branch. Merge the child branch into the parent branch first before merging the parent branch into main. Update project progress in PROJECT-SPEC.md checklist.

---
