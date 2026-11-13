# 🌿 Plantelligence — system rekomendacji roślin

Projekt edukacyjny wykorzystujący uczenie maszynowe do rekomendowania roślin na podstawie ich cech i wymagań uprawowych.

## Projekt zespołowy i mój wkład

Projekt powstał zespołowo podczas kursu Data Science w infoShare Academy.

Mój wkład obejmował przygotowanie danych, analizę eksploracyjną, tworzenie i przekształcanie cech, modelowanie oraz obliczenia służące rekomendowaniu roślin.

Aplikacja użytkowa i prezentacja projektu były odrębnymi zadaniami zespołu. To repozytorium prezentuje część analityczną i ML. Link do aplikacji nie jest obecnie dostępny.

## Cel

System umożliwia:
- wyszukiwanie roślin podobnych do wybranego gatunku;
- rekomendowanie roślin na podstawie podanego zestawu cech.

Zbiór przetworzonych danych obejmuje 159 roślin.

## Przebieg analizy

1. Eksploracja danych i przygotowanie cech.
2. Kodowanie cech kategorycznych i skalowanie wybranych cech liczbowych.
3. Redukcja wymiarowości za pomocą PCA do 10 składowych.
4. Porównanie różnych liczb klastrów KMeans metodą łokcia i Silhouette Score.
5. Wybór 10 klastrów jako kompromisu między wynikami a interpretowalnością.
6. Wyszukiwanie podobnych roślin za pomocą podobieństwa kosinusowego.

W module `recommend.py` rekomendacje są wyszukiwane w obrębie wybranego klastra.

## Technologie

Python · pandas · NumPy · scikit-learn · Jupyter Notebook · Matplotlib · joblib

## Główne pliki

- `00-EDA-Feature_Engineering.ipynb` — eksploracja i przygotowanie danych.
- `01-Feature_Transformation.ipynb` — kodowanie, skalowanie i PCA.
- `02-modelling.ipynb` — klastrowanie i analiza podobieństwa.
- `03-export_clusters.ipynb` — kod eksportu wyników klastrowania.
- `04-recommendations.ipynb` — przykłady sprawdzania modułu rekomendacji.
- `recommend.py` — funkcje rekomendacji.
- `plants_clean_final.csv` — przetworzone dane roślin.
- `artifacts/` — zapisane modele, transformacje i wektory cech.
- `requirements.txt` — zapis zależności środowiska.

## Status i ograniczenia

Dwa przykładowe wywołania modułu rekomendacji — według wybranej rośliny i według zestawu cech — zwróciły wyniki. Nie stanowi to pełnej walidacji jakości rekomendacji.

Notebooki zawierają robocze ścieżki i wymagają dostosowania przed uruchomieniem na innym komputerze. Pełne wykonanie wszystkich notebooków od początku nie zostało ponownie zweryfikowane.

Zapisane modele powstały w scikit-learn 1.7.0.

Podobieństwo cech nie gwarantuje przydatności rośliny do konkretnych warunków. Nie należy traktować rekomendacji jako potwierdzenia bezpieczeństwa rośliny dla dzieci lub zwierząt.

## Autorka części analitycznej i ML

Anna Gruszczyńska
