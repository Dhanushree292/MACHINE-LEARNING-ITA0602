import pandas as pd

df = pd.read_csv('/content/trainingdata.csv')

print("Training Data:")
print(df)

X = df.iloc[:, :-1].values
Y = df.iloc[:, -1].values

n = X.shape[1]

S = ['0'] * n
G = [['?'] * n]

for x, label in zip(X, Y):

    if label == 'Yes':
        G = [g for g in G if all(
            g[i] == '?' or g[i] == x[i] or S[i] == '0'
            for i in range(n)
        )]

        for i in range(n):
            if S[i] == '0':
                S[i] = x[i]
            elif S[i] != x[i]:
                S[i] = '?'

        G = [g for g in G if all(
            g[i] == '?' or S[i] == '?' or g[i] == S[i]
            for i in range(n)
        )]

    else:
        new_G = []

        for g in G:
            for i in range(n):
                if g[i] == '?':
                    for value in set(X[:, i]):
                        if value != x[i] and (
                            S[i] == '?' or S[i] == '0'
                            or S[i] == value
                        ):
                            new_h = g.copy()
                            new_h[i] = value
                            new_G.append(new_h)

        G = new_G

print("\nSpecific Boundary (S):")
print(S)

print("\nGeneral Boundary (G):")
for g in G:
    print(g)
