

2. KMeans – klastrowanie
from sklearn.cluster import KMeans
import matplotlib.pyplot as plt

# test różnych wartości K
inertias = []
K_range = range(2, 15)
for k in K_range:
    km = KMeans(n_clusters=k, random_state=42)
    km.fit(X_pca)
    inertias.append(km.inertia_)

# wykres Elbow
plt.plot(K_range, inertias, marker="o")
plt.xlabel("Liczba klastrów (K)")
plt.ylabel("Inertia")
plt.title("Metoda łokcia (Elbow)")
plt.show()

3. Ocena jakości – Silhouette Score
from sklearn.metrics import silhouette_score

for k in [2, 3, 4, 5, 6, 7, 8]:
    km = KMeans(n_clusters=k, random_state=42)
    labels = km.fit_predict(X_pca)
    sil = silhouette_score(X_pca, labels)
    print(f"K={k}, silhouette={sil:.3f}")

4. Finalny model KMeans
# wybieramy najlepsze K na podstawie Elbow/Silhouette
best_k = 5
kmeans = KMeans(n_clusters=best_k, random_state=42)
clusters = kmeans.fit_predict(X_pca)

df["cluster"] = clusters
df[["latin", "category_group", "cluster"]].head()

5. Cosine similarity (top-N)
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np

# macierz podobieństw
sim_matrix = cosine_similarity(X_pca)

def top_n_similar(latin_name, n=5):
    idx = df.index[df["latin"] == latin_name].tolist()[0]
    sims = sim_matrix[idx]
    similar_idx = np.argsort(-sims)
    similar_idx = [i for i in similar_idx if i != idx][:n]
    return df.loc[similar_idx, ["latin", "category_group"]].assign(similarity=sims[similar_idx].round(3))

# test
print(top_n_similar("Aeschynanthus lobianus", n=5))

🔹 Notatka (do notebooka)
### Modeling (unsupervised)
- Używamy embeddingów PCA (10D) do:
  1. **KMeans clustering** – grupowanie roślin w sensowne klastry.
  2. **Cosine similarity** – wyszukiwanie top-N podobnych roślin.
- Cel: stworzyć mechanizm rekomendacji oparty na podobieństwie wektorów roślin.
