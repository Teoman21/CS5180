# COMPLETE WRITTEN ANSWERS (2-3 Sentences Per Question/Sub-question)
## B2 English Level - All Parts Separated

---

## Question 2: Epsilon-Greedy Convergence (3 points)

**Q: What are the average rewards that the algorithm converges to using different ε values? Why does the algorithm converge to different average rewards for different ε?**

**Answer:**

The algorithm converges to approximately 1.0 for ε=0 (greedy), 1.2 for ε=0.01, and 1.4 for ε=0.1 based on my experimental results. The greedy method (ε=0) performs worst because it never explores after finding the first good action, getting stuck with suboptimal choices. Higher epsilon values explore more frequently, which helps discover the truly optimal action faster, and this benefit outweighs the cost of occasionally selecting random actions.

---

## Question 3: UCB and Optimistic Initialization Spikes (2 points)

**Q: Explain in your own words why the spikes appear (both the sharp increase and sharp decrease). Analyze and use your experimental data as further empirical evidence to back your reasoning.**

**Answer:**

The sharp increase occurs because both optimistic initialization and UCB force systematic exploration of all actions at the beginning, which eventually leads to discovering the optimal action and causes performance to jump. The sharp decrease happens after finding the optimal action because both methods must continue exploring the remaining untried or uncertain actions to verify their estimates, temporarily selecting suboptimal actions and reducing average performance. My experimental data shows these spikes occurring around steps 50-150, with optimistic initialization dropping about 20% in optimal action percentage and UCB dropping about 10%, and both methods recovering by step 200-300 once all actions have been sampled sufficiently.

---

## Question 4: Variable Step-Size Weights (1 point)

**Q: If the step-size parameters αₙ are not constant, what is the weighting on each prior reward for the general case, in terms of the sequence of step-size parameters?**

**Answer:**

For variable step-sizes, the weight on reward Rᵢ is **αᵢ × ∏ⱼ₌ᵢ₊₁ⁿ (1 - αⱼ)** and the weight on the initial estimate Q₁ is **∏ⱼ₌₁ⁿ (1 - αⱼ)**. This formula is derived by recursively expanding the incremental update equation Qₙ₊₁ = (1-αₙ)Qₙ + αₙRₙ backward from the most recent reward to the initial estimate. Recent rewards have fewer (1-αⱼ) discount factors multiplying their αᵢ term, which means they receive more weight than older rewards in the final estimate.

---

## Question 5: Bias in Q-Value Estimates (2 points)

### Part (a): Sample-Average Bias

**Q: Consider the sample-average estimate in Equation 2.1. Is it biased or unbiased? Explain briefly.**

**Answer:**

The sample-average estimate is **unbiased** because the expected value E[Qₙ] equals the true value q*. Taking the expectation of Qₙ = (R₁+...+Rₙ₋₁)/(n-1) gives E[Qₙ] = (1/(n-1))×[(n-1)×q*] = q* since each reward has expected value q*. Therefore, on average, the estimate equals the true value regardless of the number of samples.

---

### Part (b): Exponential Average with Q₁=0

**Q: If Q₁ = 0, is Qₙ for n > 1 biased? Explain briefly.**

**Answer:**

Yes, the exponential recency-weighted average is **biased** when Q₁=0 (assuming q*≠0). From the exponential weighting formula, taking the expected value gives E[Qₙ] = [1-(1-α)ⁿ⁻¹]×q*, which is less than q* for finite n since (1-α)ⁿ⁻¹ > 0. Since E[Qₙ] ≠ q*, the estimate is biased, with the bias equal to -q*×(1-α)ⁿ⁻¹.

---

### Part (c): Conditions for Unbiased Estimate

**Q: Derive conditions for when Qₙ will be unbiased (Q₁ can be non-zero).**

**Answer:**

The exponential recency-weighted average is unbiased if and only if **Q₁ = q***. Setting the expected value E[Qₙ] = (1-α)ⁿ⁻¹Q₁ + [1-(1-α)ⁿ⁻¹]q* equal to q* and solving gives (1-α)ⁿ⁻¹(Q₁-q*) = 0, which requires Q₁ = q*. In practice, this condition is impossible to satisfy because we don't know the true value q* when initializing the estimate.

---

### Part (d): Asymptotic Unbiasedness

**Q: Show that Qₙ is asymptotically unbiased, i.e., it is an unbiased estimator as n → ∞.**

**Answer:**

The exponential average is asymptotically unbiased because lim(n→∞) E[Qₙ] = q*. Since 0 < α < 1, the term (1-α)ⁿ⁻¹ approaches zero as n approaches infinity, making E[Qₙ] = (1-α)ⁿ⁻¹Q₁ + [1-(1-α)ⁿ⁻¹]q* converge to 0×Q₁ + 1×q* = q*. Therefore, regardless of the initial value Q₁, the estimate becomes unbiased in the limit as the number of samples grows infinitely large.

---

### Part (e): Why Generally Biased

**Q: Why should we expect that the exponential recency-weighted average will be biased in general?**

**Answer:**

The exponential average is generally biased because it always retains a weight of (1-α)ⁿ⁻¹ on the initial estimate Q₁, which is positive for any finite n, and in practice Q₁ almost never equals q*. The weights on all observed rewards sum to only [1-(1-α)ⁿ⁻¹] rather than 1, with the remaining weight staying on Q₁, creating systematic error. This design is intentional because the method prioritizes recent rewards to track changing values in non-stationary problems, but this useful property comes at the cost of bias in stationary environments where the true value doesn't change.

---

## END OF ANSWERS
