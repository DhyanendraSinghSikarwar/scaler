# ML Notes

## Q1. In linear regression, when to use L1 vs L2 regularization?

**Context:** Regularization is used when the model overfits. It adds a penalty on coefficient size to the loss.

| | L1 (Lasso) | L2 (Ridge) |
|---|---|---|
| Penalty | λ Σ\|w\| | λ Σw² |
| Effect on weights | Pushes some weights to exactly 0 | Shrinks all weights, rarely to 0 |
| Feature selection | Yes (built-in) | No |
| Correlated features | Picks one, drops others (unstable) | Spreads weight across them (stable) |
| Solution | No closed form | Closed form exists |

**Use L1 when:**
- Many features, only a few are expected to matter
- You want a sparse, interpretable model
- You want automatic feature selection

**Use L2 when:**
- Most features contribute a little
- Features are highly correlated (multicollinearity)
- You want stable coefficients

**Use Elastic Net (L1 + L2) when:**
- Many features + correlated groups
- Features > samples (p > n)

**Points to remember:**
- λ (alpha) controls strength: higher λ = simpler model = more bias, less variance
- Choose λ with cross-validation
- Scale/standardize features before regularizing (penalty is scale-sensitive)
- Intercept is not regularized
- Default choice if unsure: L2 (Ridge)

---

## Q2. How does L2 help with correlated features?

**Problem without regularization:**
- If x1 ≈ x2, many weight combos give the same prediction (e.g., w1=100, w2=-98 ≈ w1=1, w2=1)
- XᵀX becomes nearly singular → huge, unstable, sign-flipping coefficients
- Small data changes → big coefficient swings (high variance)

**How L2 fixes it:**
- Penalty Σw² makes large opposing weights expensive
- Among equal-fit solutions, the smallest-norm one wins → weight split evenly (w1 ≈ w2)
  - e.g., 1² + 1² = 2 is cheaper than 2² + 0² = 4
- Math: w = (XᵀX + λI)⁻¹ Xᵀy → adding λI makes the matrix invertible and well-conditioned

**Why L1 behaves differently:**
- |1| + |1| = |2| + |0| → L1 is indifferent, so it arbitrarily picks one feature and zeros the other

**Points to remember:**
- L2 = grouping effect (correlated features get similar weights)
- L2 reduces variance, not the correlation itself
- L2 keeps all features; it doesn't drop redundant ones

---

## Q3. Why can't we use MSE for logistic regression?

**Convexity summary:**

| Model | Loss | Shape |
|---|---|---|
| Linear regression | MSE | Convex ✅ |
| Logistic regression | MSE | Non-convex ❌ |
| Logistic regression | Log loss (BCE) | Convex ✅ |

**Reason 1: Non-convexity**
- ŷ = σ(wᵀx) is non-linear; squaring (y − σ(z)) creates a wavy loss surface
- Multiple local minima and flat regions → gradient descent may get stuck
- Log loss with sigmoid is convex → one global minimum

**Reason 2: Vanishing gradients**
- MSE gradient: (ŷ − y) · σ'(z) · x
  - σ'(z) ≈ 0 when z is large, even if the prediction is badly wrong → learning stalls
- Log loss gradient: (ŷ − y) · x
  - σ'(z) cancels out → gradient stays strong when wrong

**Reason 3: Weak penalty for confident mistakes**
- y = 1, ŷ = 0.001 → MSE ≈ 1 (bounded)
- Same case → log loss = −log(0.001) ≈ 6.9 (→ ∞ as ŷ → 0)

**Reason 4: Probabilistic view**
- MSE = MLE assuming Gaussian noise (fits continuous targets)
- Log loss = MLE assuming Bernoulli outputs (fits 0/1 targets)

**Formula:**
- BCE = −[y log(ŷ) + (1 − y) log(1 − ŷ)]

---

## Q4. Imbalanced data: balance first then use PR AUC, or use ROC AUC directly?

**Answer:** Neither. Use **PR AUC directly on the original (imbalanced) test set**. Never balance the test/validation set.

**Why not balance the test set:**
- Test set must reflect the real-world class ratio
- PR AUC depends on class ratio → balancing inflates precision → falsely optimistic score
- Balancing only applies (optionally) to **training data**

**Why ROC AUC misleads on imbalance:**
- FPR = FP / (FP + TN); huge TN count keeps FPR tiny even with many FPs
- ROC AUC is insensitive to class ratio → looks good even when the model is poor on the minority class
- Not "wrong", just over-optimistic for the rare class

**Why PR AUC works:**
- Precision = TP / (TP + FP) → ignores TN, directly exposes false positives
- Focuses on the minority (positive) class

**Correct workflow:**
1. Split data (stratified) → keep test set imbalanced
2. Handle imbalance on train only (class_weight, SMOTE, undersampling), if needed
3. Evaluate with PR AUC (+ precision, recall, F1 at chosen threshold)

**Points to remember:**
- PR AUC baseline = positive class ratio (e.g., 5% positives → random model ≈ 0.05)
- ROC AUC baseline = 0.5 regardless of imbalance
- Resample after the split, never before (avoids leakage)
- Use stratified K-fold for CV

---

## Q5. Can we use linear regression for non-linear data?

**Answer:** Yes. Linear regression must be linear in the **weights**, not in the features.

**How:** transform the features, then fit a normal linear regression
- Polynomial features: x, x², x³ → y = w0 + w1x + w2x²
- Log / sqrt / exp transforms: log(x), √x
- Interaction terms: x1·x2
- Splines / binning for piecewise patterns

**Points to remember:**
- Still solved with OLS → remains convex, closed form
- Higher polynomial degree → overfitting risk → add L1/L2 regularization
- Scale features after polynomial expansion
- Check residual plots: a curved pattern means non-linearity is missing
- If the relationship is very complex, use tree models / neural nets instead

---

## Q6. For how many features should we use KNN?

**Answer:** There's no fixed number. KNN works best with **low dimensions, roughly < 10–20 features**, and performance drops as features increase.

**Why: curse of dimensionality**
- In high dimensions, all points become nearly equally far apart → the "nearest" neighbor isn't really near
- The data needed to cover the space grows exponentially with the number of features
- Irrelevant features add noise to the distance

**When KNN fits:**
- Few features, enough samples (n ≫ d)
- Small/medium dataset (prediction is slow: it computes distance to all points)

**If many features:**
- Reduce dimensions first: PCA, feature selection
- Or switch to trees / linear models

**Points to remember:**
- Always scale features (distance-based)
- Choose k with cross-validation; odd k for binary classification
- Small k → overfit; large k → underfit
- No training phase (lazy learner); cost is paid at prediction time

---

## Q7. Decision Tree: is standardizing features useful?

**Answer:** No.
- Trees split on thresholds (x < t); scaling just moves t, so the splits stay the same
- Any monotonic transform (scaling, log) leaves the tree unchanged
- Applies to Random Forest, XGBoost, LightGBM too
- Only scale if the same pipeline also feeds a distance/gradient-based model (KNN, SVM, linear)

---

## Q8. Decision Tree: is one-hot encoding categorical features useful?

**Answer:** Some encoding is required (sklearn needs numbers), but OHE is often **not the best choice**.

| Encoding | For trees |
|---|---|
| One-hot | OK for low cardinality (< ~10–15 categories) |
| Label / ordinal | Often works fine; tree can isolate values with multiple splits |
| Native categorical | LightGBM, CatBoost, sklearn HistGradientBoosting handle it directly (best) |

**Downsides of OHE in trees:**
- Each split separates only one category vs. the rest → deeper, unbalanced trees
- Many sparse columns → the feature's importance gets split across columns
- Slower with high cardinality

---

## Q9. Decision Tree: is target encoding useful for high-cardinality categorical columns?

**Answer:** Yes, very useful.
- Replaces each category with the mean target for that category → 1 compact numeric column
- Avoids the column explosion of OHE
- Captures the category–target relationship directly

**Risk: target leakage / overfitting**
- Rare categories get noisy means that "memorize" the target

**Fixes:**
- Out-of-fold (K-fold) target encoding: compute encodings on other folds
- Smoothing: blend the category mean with the global mean (weight by count)
- Fit the encoder on train only, then apply to test
- Alternatives: frequency/count encoding, CatBoost (built-in ordered target encoding)

---

## Q10. Random Forest: at each split, do we sample rows as well as columns?

**Answer:** No. **Rows are sampled once per tree; columns are sampled at every split.**

| | When sampled | Detail |
|---|---|---|
| Rows | Once per tree (bootstrap) | n rows drawn with replacement |
| Columns | At every split (node) | Random subset of size `max_features` |

**Flow for one tree:**
1. Draw a bootstrap sample of rows → this is the tree's full dataset
2. At the root: pick random subset of columns → best split among them
3. Rows are divided into child nodes
4. At each child: use **only the rows that reached that node** (no new row sampling) + draw a **fresh random column subset**
5. Repeat until stopping criteria (max_depth, min_samples_leaf, pure node)

**Points to remember:**
- Best split = highest information gain (lowest weighted child impurity), not just low entropy of one node
- sklearn default criterion = Gini (entropy optional)
- `max_features` default: √p for classification, p (all features) for regression in recent sklearn versions
- ~37% of rows are left out of each bootstrap → **Out-of-Bag (OOB)** samples → free validation (`oob_score=True`)
- Per-split column sampling decorrelates the trees → more variance reduction
- XGBoost/LightGBM differ: `colsample_bytree` (per tree), `colsample_bylevel`, `colsample_bynode`; row `subsample` is per tree

---

## Q11. Gradient Boosting: residual vs pseudo-residual

**Residual:** rᵢ = yᵢ − F(xᵢ) (actual − predicted)

**Pseudo-residual:** rᵢ = −∂L(yᵢ, F(xᵢ)) / ∂F(xᵢ), evaluated at F = F_{m−1}
- Negative gradient of the loss w.r.t. the **current combined prediction** (all previous models together), not just the last model
- Used for **every** loss (regression and classification); it's what makes boosting "gradient" boosting

**Algorithm (general):**
1. F₀ = constant that minimizes the loss
2. For m = 1 … M:
   - Compute pseudo-residuals rᵢ using F_{m−1}
   - Train tree hₘ on (X, r)
   - Update: Fₘ = F_{m−1} + η·hₘ (η = learning rate)
3. Final prediction = F_M

**Pseudo-residual by loss:**

| Task | Loss | F₀ (baseline) | Pseudo-residual |
|---|---|---|---|
| Regression | MSE: ½(y − F)² | Mean of y | y − F (= residual) |
| Regression | MAE: \|y − F\| | Median of y | sign(y − F) |
| Classification | Log loss | log(p / (1 − p)) (log-odds) | y − p, where p = σ(F) |

**Corrections to common misunderstandings:**
- Pseudo-residual = residual **only for MSE**, not for all regression losses (MAE, Huber differ)
- Pseudo-residuals are not exclusive to classification
- Residual uses the full ensemble with learning rate: r = y − (F₀ + η·h₁), not y − (m₀ + h₁)
- Classification baseline is log-odds, not the mean; F lives in log-odds space, and σ(F) gives probability

**Points to remember:**
- Each tree fits the negative gradient → boosting = gradient descent in function space
- Small η + more trees → better generalization
- In classification, leaf values are adjusted (Newton step), not just the mean of pseudo-residuals

---

## Q12. Naive Bayes: when to use Multinomial NB? What about mixed feature types?

**NB types (sklearn):**

| Type | Feature type | Typical use |
|---|---|---|
| GaussianNB | Continuous, assumed normal per class | Numeric data (iris, sensor data) |
| MultinomialNB | Non-negative **counts / frequencies** | Text: word counts, TF-IDF |
| BernoulliNB | Binary (0/1), presence/absence | Short text with binary word presence, binary flags |
| CategoricalNB | Categorical (label-encoded integers) | Categorical columns (color, city) |
| ComplementNB | Counts | Text with imbalanced classes |

**Multinomial NB:**
- Use when features are counts of events (e.g., how many times a word appears)
- Values must be non-negative; works with TF-IDF in practice
- Bernoulli vs Multinomial: Bernoulli explicitly penalizes **absence** of a feature; Multinomial uses frequency and ignores absence

**Correction:** Gaussian NB isn't just for "numerical" columns; it assumes each feature is **normally distributed within each class**. Skewed features → transform (log, Box-Cox) first.

**Mixed data (OHE + numerical + categorical):** no single NB handles it. Options:

1. **Combine separate NBs (most correct)**
   - GaussianNB → numeric columns
   - CategoricalNB → categorical columns (label-encoded; prefer this over OHE)
   - BernoulliNB → binary columns
   - Combine log-probabilities (valid because of the independence assumption):
     - log P(y|x) = Σ log Pₘ(y|xₘ) − (M − 1)·log P(y) + const
     - M = number of models; subtract the extra copies of the prior
2. **Discretize numeric columns** (KBinsDiscretizer) → everything categorical → single CategoricalNB
3. **Switch models**: tree-based models handle mixed types naturally

**Points to remember:**
- OHE columns of one feature are dependent (only one can be 1) → violates the naive assumption; CategoricalNB is better
- Use Laplace smoothing (`alpha`) to avoid zero probabilities
- NB gives good rankings/classes but poorly calibrated probabilities
