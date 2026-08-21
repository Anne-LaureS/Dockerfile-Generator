# Empaquette l'outil lui-même (le script générateur Python), pas un Dockerfile généré.
# Volontairement dans docker/ plutôt qu'à la racine : le générateur écrit un fichier nommé
# "Dockerfile" dans le dossier courant — le placer à la racine créerait une collision si
# quelqu'un exécute le générateur depuis là.
FROM python:3.11-slim

# Le script vit à part de /output pour qu'un volume monté sur /output (pour récupérer les
# fichiers générés sur l'hôte) n'écrase jamais le script lui-même.
WORKDIR /opt/generator
COPY feature/python-script/generate_dockerfile.py .

# /output appartient à l'utilisateur non-root : c'est là que le script écrit ses fichiers
# (chemins relatifs au dossier courant), et là que l'utilisateur montera son volume.
RUN mkdir /output && chown 1000:1000 /output
WORKDIR /output
USER 1000:1000

ENTRYPOINT ["python", "/opt/generator/generate_dockerfile.py"]
