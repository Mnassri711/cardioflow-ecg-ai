# CardioFlow ECG AI

Prototype d'intelligence artificielle pour la classification multilabel d'ECG 12 derivations. Le projet utilise un sous-ensemble reel de **PTB-XL**, entraine un **CNN 1D** dans Google Colab, mesure ses performances et produit une explication simple par carte de saillance.

> Projet pedagogique et de recherche uniquement. Ce prototype n'est pas un dispositif medical et ne doit jamais etre utilise pour diagnostiquer ou traiter un patient.

## Ce que le notebook realise

1. telecharge les metadonnees officielles PTB-XL et un sous-ensemble configurable de signaux 100 Hz ;
2. transforme les annotations SCP-ECG en cinq superclasses : `NORM`, `MI`, `STTC`, `CD`, `HYP` ;
3. respecte la separation officielle par patients : folds 1-8 / 9 / 10 ;
4. entraine un reseau convolutif 1D multilabel ;
5. calibre un seuil par classe sur la validation ;
6. rapporte AUROC, AUPRC et F1 par classe ;
7. visualise les douze derivations et une carte de saillance ;
8. lance une mini-demo Gradio dans Colab.

## Execution dans Google Colab

1. Ouvrir [`notebooks/CardioFlow_ECG_AI.ipynb`](notebooks/CardioFlow_ECG_AI.ipynb) dans GitHub.
2. Cliquer sur **Open in Colab**, ou importer manuellement le notebook dans [Google Colab](https://colab.research.google.com/).
3. Dans Colab : `Runtime > Change runtime type > T4 GPU`.
4. Executer les cellules dans l'ordre.
5. Pour une premiere execution rapide, conserver `TRAIN_SAMPLES = 1200`, `VAL_SAMPLES = 200`, `TEST_SAMPLES = 200` et `EPOCHS = 6`.
6. A la fin, telecharger `cardioflow_cnn.keras`, `metrics.json` et `thresholds.json`.

Temps indicatif avec un GPU Colab : 15 a 35 minutes, selon le debit de telechargement de PhysioNet.

## Ce qu'il faut savoir expliquer

- Pourquoi le probleme est **multilabel** : un ECG peut appartenir a plusieurs superclasses.
- Pourquoi les folds sont separes par patient : eviter qu'un meme patient apparaisse en entrainement et en test.
- Pourquoi on calibre les seuils sur la validation et jamais sur le test.
- Ce qu'une carte de saillance indique, et surtout ce qu'elle ne prouve pas cliniquement.
- Pourquoi AUROC seul ne suffit pas sur des classes desequilibrees ; AUPRC et F1 sont egalement rapportes.

## Structure

```text
cardioflow-ecg-ai/
|-- notebooks/
|   `-- CardioFlow_ECG_AI.ipynb
|-- .gitignore
|-- LICENSE
|-- README.md
`-- requirements-colab.txt
```

## Donnees et citation

Les donnees ne sont pas stockees dans ce depot. Elles sont telechargees depuis PhysioNet pendant l'execution.

- Wagner, P. et al. (2020), *PTB-XL: A Large Publicly Available ECG Dataset*, Scientific Data.
- Dataset PTB-XL v1.0.3 : https://physionet.org/content/ptb-xl/1.0.3/
- Licence des donnees : Creative Commons Attribution 4.0.

## Ameliorations possibles

- entrainer sur l'integralite de PTB-XL ;
- comparer le CNN avec une baseline logistique et un ResNet-1D ;
- ajouter Integrated Gradients ou Grad-CAM 1D ;
- exporter le modele vers une API FastAPI ;
- suivre les experiences avec MLflow.

## Licence

Le code est publie sous licence MIT. PTB-XL conserve sa propre licence CC BY 4.0.
