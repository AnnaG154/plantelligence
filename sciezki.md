project/
│
├── data/
│   └── plants_clean_final.csv
├── artifacts/
│   └── plant_embeddings.npy
└── notebooks/
    └── 03_modeling_unsupervised.ipynb

| Gdzie jest plik?                           | Ścieżka do użycia w notebooku (`notebooks/`) | Przykład kodu                                        |
| ------------------------------------------ | -------------------------------------------- | ---------------------------------------------------- |
| **W tym samym folderze co notebook**       | `"plik.csv"`                                 | `df = pd.read_csv("plik.csv")`                       |
| **W folderze obok (`data/`)**              | `"../data/plants_clean_final.csv"`           | `df = pd.read_csv("../data/plants_clean_final.csv")` |
| **W folderze obok (`artifacts/`)**         | `"../artifacts/plant_embeddings.npy"`        | `X = np.load("../artifacts/plant_embeddings.npy")`   |
| **W katalogu głównym projektu (project/)** | `"../plik.csv"`                              | `df = pd.read_csv("../plik.csv")`                    |
| **Dwa poziomy wyżej**                      | `"../../plik.csv"`                           | `df = pd.read_csv("../../plik.csv")`                 |
🔑 Zasady

./ = bieżący katalog (można pominąć).

../ = wyjście jeden poziom wyżej.

Każde dodatkowe ../ = kolejny poziom wyżej.

Ścieżki zawsze w cudzysłowie.