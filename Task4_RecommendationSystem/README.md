import pandas as pd
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np

# Sample data
data = {'User': ['A','A','B','B','C'], 'Item': ['Item1','Item2','Item2','Item3','Item1']}
df = pd.DataFrame(data)

# Pivot table
pivot = df.pivot_table(index='User', columns='Item', aggfunc=len, fill_value=0)

# Cosine similarity
sim = cosine_similarity(pivot)
print("User similarity:\n", sim)

# Recommend for User A
user_index = 0
recommendations = np.argsort(sim[user_index])[::-1][1:]  # skip self
print("Recommendations for User A:", [pivot.index[i] for i in recommendations])
