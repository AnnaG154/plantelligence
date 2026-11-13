### DoD — Feature Transformation

## Zakres (co obejmuje etap)

- OHE: tempmin_pasmo, tempmax_pasmo, category_group, light_level_clean, watering_group → 18 kolumn

 - Scaler: tylko is_cold_tolerant, is_heat_tolerant (2 kolumny)

 - Binarka: toxicity_any (bez skalowania, 0/1)

 - Sklejony wektor inference: 21 cech (18 OHE + 2 scaled + 1 bin)

 - PCA: embedding 10D (z 21 cech)

## Artefakty (muszą istnieć)

 - artifacts/ohe.pkl (OneHotEncoder, handle_unknown="ignore")

 - artifacts/feature_names_ohe.json (dokładnie 18 nazw)

 - artifacts/scaler.pkl (fit na 2 kolumnach)

 - artifacts/pca.pkl (PCA n_components=10, random_state=42)

 - artifacts/plant_embeddings.npy (kształt: 159×10)

 - artifacts/plant_embeddings.csv (embedding + latin, category_group)

 - feature_list.json (źródło prawdy dot. kolejności)

## Testy / asercje (muszą przejść)

 - OHE smoke test: 18 kolumn, brak NaN w kategoriach, nazwy z get_feature_names_out() == feature_names_ohe.json

 - Scaler smoke test: średnia ~0, std ~1 dla 2 kolumn

 - Inference builder: build_feature_matrix() → shape = (n, 21), kolejność = OHE→scaled→bin

 - PCA fit: explained_variance_ratio_ ~96% (10 składowych)

 - Cosine similarity sanity-check: top-N podobnych dla kilku losowych roślin wygląda sensownie (bez „losowych” miksów)

## Reproducibility / wersjonowanie

 - Kod notatnika zawiera minimalne notatki MD (co, po co, wynik)

 - Wszystkie ścieżki względne (artifacts/…, plants_clean_final.csv)

 -  wersji pakietów (np. pip freeze > artifacts/requirements.txt) — zalecane

 - Utrzymana kolejność cech zgodnie z feature_list.json (źródło prawdy dla apki)

## Akceptacja biznesowa (kryteria)

 - Na wejściu apki można zbudować stabilny wektor 21 cech dla dowolnej rośliny/odpowiedzi użytkownika

 - Dostępne embeddingi 10D dla całej bazy (ładowane z plant_embeddings.npy)

 - Cosine similarity zwraca sensowne top-N → materiał do rekomendacji

## Ryzyka / uwagi

- OHE zależy od słownika kategorii → zabezpieczone handle_unknown="ignore"

- feature_list.json musi pozostać spójny z backendem i UI (kolejność!)

Gotowe snippet-y (do README/notebooka)

Inference 21 cech: OHE (18) + scaler (2) + bin (1)

Embedding: to_embedding(df) → (n, 10)

Similarity: cosine_similarity(X_pca) + funkcja top_n_similar()

JIRA – komentarz zamknięcia tasku (krótki)

Done: OHE(18) + Scaler(2) + Bin(1) → 21 cech; PCA→10D; artefakty zapisane (ohe.pkl, feature_names_ohe.json, scaler.pkl, pca.pkl, plant_embeddings.npy). Testy smoke OK (kształty, nazwy, EVR~96%, sanity cosine). Kolejność cech zgodna z feature_list.json. Etap Feature Transformation zakończony.

Następny krok (kolejny etap)

Start Modeling (unsupervised):

 KMeans na embeddingach 10D (wybór K: Elbow/Silhouette)

 Similarity search: endpoint „top-N podobnych” (cosine)

 Integracja z quizem (mapowanie odpowiedzi → 21 cech → 10D → rekomendacje)


 ### ✅ Definition of Done (DoD) — Modeling (Plantelligence)
## Zakres (co obejmuje etap)

 Wczytanie embeddingów PCA (10D) z plant_embeddings.npy.

 KMeans clustering na embeddingach.

 Analiza jakości klastrów (Elbow + Silhouette).

 Interpretacja klastrów (crosstab kategorii vs. klastry).

 Cosine similarity jako silnik rekomendacji (Top-N podobnych).

 Zapis modelu KMeans (kmeans.pkl) jako artefakt.

## Artefakty

 artifacts/kmeans.pkl — finalny model KMeans (np. K=10).

 artifacts/plant_embeddings.npy — embeddingi roślin (159 × 10).

 (opcjonalnie) artifacts/plant_embeddings.csv — embeddingi z metadanymi (debug).

## Testy / asercje

 Elbow: krzywa inercji sprawdzona dla 2–20 klastrów.

 Silhouette Score: rosnący trend, max ~0.55 przy K=20.

 Interpretacja: klastery dobrze odwzorowują naturalne grupy (np. klastry palm, sukulentów, paproci).

 Cosine similarity: zwraca sensowne Top-N podobnych roślin (przykład: Aeschynanthus lobianus → Tillandsia, Vriesea, palma).

## Reproducibility

 Wszystkie ścieżki względne (artifacts/…, data/…).

 Wyniki i wnioski opisane w notebooku (notatki MD).

 requirements.txt zapisany w artifacts/ (spójność środowiska).

## Akceptacja biznesowa

 Możliwość podziału bazy na sensowne klastry roślin.

 Możliwość zwrócenia rekomendacji Top-N podobnych gatunków dla dowolnej rośliny.

 Pipeline inference = stabilny: quiz → 21 cech → PCA → KMeans/Cosine → rekomendacje.

Komentarz do JIRA (krótki)

Done: Modeling ukończony. KMeans (K=10) wytrenowany i zapisany (kmeans.pkl), embeddingi w plant_embeddings.npy. Analiza Elbow+Silhouette przeprowadzona, klastry zinterpretowane. Cosine similarity działa jako silnik rekomendacji. Etap Modeling zakończony.


### 🧾 Definition of Done (DoD) – Etap: Modeling (Plantelligence)

Cel etapu:
Doprowadzenie modułu rekomendacyjnego recommend.py do pełnej funkcjonalności – generowania trafnych rekomendacji roślin na podstawie cech użytkownika lub nazwy łacińskiej.

✅ Checklist – zakończenie etapu Modeling

1. Artefakty techniczne

 Wszystkie pliki modeli zapisane i spójne (ohe.pkl, scaler.pkl, pca.pkl, kmeans.pkl, embeddings.npy, feature_list.json).

 Plik recommend.py naprawiony (usunięto błędy składni i wcięć).

 Poprawne wczytywanie feature_list.json (uwzględniono strukturę features_order).

 Folder __pycache__ zweryfikowany – nie stanowi błędu, zawiera pliki .pyc.

2. Transformacje danych

 Funkcja _to_embedding_from_features() odtwarza dokładny układ cech:
[OHE (5)] + [Scaler (2)] + [Binarka (1)].

 Liczba cech zgodna z pipeline’em treningowym (18 kolumn).

 Znormalizowane dane wejściowe generują poprawne embeddingi PCA.

3. Testy i weryfikacja

 Funkcja recommend_by_answers() działa – zwraca sensowne dopasowania.

 Funkcja recommend_by_plant() działa – poprawnie wyklucza roślinę źródłową.

 Liczba wektorów i etykiet zgodna:

Wiersze: 159

Embeddingi PCA: 159

Klastry: 159

4. Wynik etapu
✅ Moduł rekomendacji działa poprawnie.
✅ Model przeszedł pełny cykl od czyszczenia do embeddingów.
✅ Gotowy do integracji z quizem użytkownika w aplikacji webowej.

Status: 🟢 Etap „Modeling” zakończony – można przejść do etapu „Integration (Quiz & App UI)”