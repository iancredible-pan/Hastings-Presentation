# Hastings (1970): Speaker Notes

**Two presenters · 42-minute teaching presentation · English throughout**

Presenter A covers slides 1–9, from 00:00 to 21:00. Presenter B covers slides 10–18, from 21:00 to 42:00. Replace the presenter labels with your names. Bracketed directions are stage cues, not spoken text. The timing includes reading equations, audience responses, and deliberate pauses; rehearse aloud instead of treating the script as continuous reading.

**Reading:** W. K. Hastings, “Monte Carlo sampling methods using Markov chains and their applications,” *Biometrika* 57(1), 97–109 (1970). Assigned scope: Section 1 and Sections 2.1–2.2, pp. 97–101. The two-state example is a teaching example created for this presentation, not a numerical example from Hastings's paper.

## Slide 1 — Sampling by designing a Markov chain

**Presenter A · 1:00 · 00:00–01:00**

Today we are discussing Hastings's 1970 paper on Monte Carlo sampling using Markov chains. Our presentation focuses on one question: how can we construct a Markov chain whose stationary distribution is a distribution that we choose?

We will build one small example all the way through. We will choose the target, propose possible moves, decide which moves to accept, and check the resulting transition matrix.

The central idea is that we do not have to generate independent samples directly from a complicated distribution. We can design a sequence of dependent samples that preserves that distribution. The important word here is “design.” We are choosing how the chain moves so that it has the probability distribution we want.

[Pause. Point to the central question before advancing.]

## Slide 2 — We want expectations. We need samples.

**Presenter A · 2:30 · 01:00–03:30**

Let us begin with the problem that motivates the paper. In statistics, we often want an expectation under a probability distribution. We write this as the integral of a function f of x multiplied by a density p of x.

For example, if f of x equals x, the expectation is the mean. If f is an indicator of an event, the expectation is the probability of that event. So the same mathematical problem covers many useful questions.

Sometimes we can evaluate the integral analytically. But for a complicated distribution, especially in many dimensions, this can be difficult. Monte Carlo offers another route: generate samples, evaluate the function at those samples, and average the results.

That sounds straightforward, but it moves the difficulty to a new place. How do we generate the samples? Being able to evaluate a density at a point does not necessarily mean that we can easily draw a random point from that density.

Hastings begins by discussing existing approaches, including factorization, importance sampling, and simulation through transformations. We will not compare all of them today. The motivation we need is that direct sampling can be difficult even when evaluating relative probabilities is manageable.

[Point to the expectation and then to the sampling step.]

The practical question is: can we estimate this expectation using a sampling mechanism that is easier to construct?

## Slide 3 — Keep the average. Change the sampling mechanism.

**Presenter A · 2:30 · 03:30–06:00**

With ordinary Monte Carlo, we imagine independent draws from the target distribution. We estimate the expectation by taking the average of f evaluated at those draws.

Hastings keeps the idea of averaging, but changes the way the observations are generated. Instead of requiring a fresh independent draw every time, we generate a sequence using a Markov chain.

The Markov property means that the distribution of the next state depends on the current state. Once we know the current state, the earlier history supplies no additional information about the next transition.

This does not mean that the samples are independent. In fact, the mechanism usually creates dependence between nearby observations. If the chain is currently in one region, the next observation may be more likely to remain near that region.

Under appropriate conditions, the average along the chain still estimates the target expectation. In the finite-state setting we will use, irreducibility gives the relevant long-run averaging result. We will return to that check near the end.

There is also a cost: dependence changes the uncertainty of the average. We cannot automatically use the standard error formula for independent samples. The paper discusses that issue in Section 2.1, but our main focus is the construction of the sampling mechanism.

[Pause at the shared averaging formula.]

The estimator has a familiar form. Our task is to make sure the chain producing its observations is suitable.

## Slide 4 — One target will guide the entire construction.

**Presenter A · 2:00 · 06:00–08:00**

Here is the example that will stay with us throughout the presentation. There are only two states, labelled zero and one. We assign them unnormalized weights of three and one.

After normalization, the target probabilities are three quarters and one quarter. We can normalize these two numbers immediately, of course. We use such a small example because we want every calculation to be visible. Later we will see why the algorithm does not need to perform this normalization.

What should a successful chain do? Over a long run, it should spend approximately three quarters of its time in state zero and one quarter in state one.

That is a statement about long-run proportions. It does not mean that every four observations must contain exactly three zeros and one one. The sequence is random, and a short segment can have different proportions.

There are many possible transition mechanisms. Always switching states is one possibility. Sometimes staying in place is another. Our question is which mechanism produces the target probabilities.

[Ask the audience to keep the ratio three to one in mind.]

We know the distribution we want. We still have to construct the movement rule.

## Slide 5 — A stationary distribution survives one transition.

**Presenter A · 3:00 · 08:00–11:00**

For a finite-state Markov chain, the movement rule is described by a transition matrix P. The entry p sub i j is the probability that the next state is j, given that the current state is i.

Each row represents the possible outcomes starting from one state. Every entry must be nonnegative, and every row must sum to one. These are ordinary probability requirements.

Now imagine that the current state is random, with a row-vector distribution mu. After one transition, its distribution is mu times P. We get the probability of ending in j by adding the contributions from every possible starting state.

We call pi stationary if pi times P equals pi. If the chain already has distribution pi, taking another step leaves that distribution unchanged.

[Read the equation aloud: “Pi P equals pi.”]

Stationarity does not mean the state itself stays fixed. Individual paths can keep moving while the distribution across possible states remains unchanged. For example, some probability can flow out of state zero while the same amount flows back into it.

This also does not say that a chain started at a particular state immediately has distribution pi. That is a separate question about how the distribution evolves from its starting point. The instructor will develop that distinction further.

For our construction, the equation pi P equals pi is the target we want our transition matrix to satisfy. Solving it directly is possible in a tiny example. Hastings gives us a reusable way to build a matrix that satisfies it.

## Slide 6 — Balance probability flow in both directions.

**Presenter A · 2:30 · 11:00–13:30**

The useful condition is called detailed balance, or reversibility. It says that pi i times p i j equals pi j times p j i.

To interpret the left side, first select a current state according to pi. The probability of starting at i is pi i. The conditional probability of then moving to j is p i j. Their product is the probability flow from i to j under the target distribution.

Detailed balance matches that flow with the reverse flow from j to i.

Notice that we are balancing weighted flows, not requiring the two transition probabilities to be equal. A common state can have a smaller chance of leaving, while a less common state has a larger chance of moving back.

Why does this help? Fix a destination j and sum the balance equation over all starting states i. The incoming probability is pi j times the sum of row j. That row sums to one, so the incoming probability is exactly pi j.

[Point to the stationarity statement beneath the flow diagram.]

This proves stationarity. Detailed balance is a sufficient condition, but it is not necessary: a stationary chain need not be reversible. We use it because it gives us a convenient construction method. We do not need a full classification of stationary chains for today's example.

## Slide 7 — A convenient proposal may have the wrong target.

**Presenter A · 2:30 · 13:30–16:00**

Now let us propose a very simple way to generate candidate moves. If we are at zero, propose one. If we are at one, propose zero. The proposal matrix Q therefore has zeros on its diagonal and ones off the diagonal.

It is important to distinguish Q from the transition matrix we ultimately want. Q tells us how to suggest a destination. It does not automatically give the correct distribution.

What happens if we accept every proposal? Starting from zero, we get one, zero, one, zero, and so on. The chain always switches. Its long-run occupation proportions are one half and one half, rather than our desired three quarters and one quarter.

The uniform distribution is stationary for this proposal matrix. Starting from a fixed state also illustrates periodicity: the distribution alternates instead of settling down at each time step. We do not need to explore that theory here. The immediate problem is that the proposal alone does not preserve our desired target.

[Ask: “Which state should the final mechanism allow us to remain in more often?” Pause for an answer.]

State zero needs more occupation time. We can achieve this by sometimes refusing a proposed move out of zero. That introduces the second part of the construction: acceptance and rejection.

## Slide 8 — A proposal becomes a move only if we accept it.

**Presenter A · 3:00 · 16:00–19:00**

Hastings separates a transition into two stages. First, propose a state j using row i of Q. Second, accept that candidate with probability alpha i j.

For different states i and j, the actual transition probability is q i j multiplied by alpha i j. We have to propose the move and accept it in order for the chain to make that move.

If the proposal is rejected, the next state is the current state again. The iteration still happened, and the chain still has a next observation. That observation simply repeats the current value.

The diagonal entry of P is one minus the sum of its off-diagonal entries. This makes the row sum to one. In general, remaining at the same state can also result from proposing that state itself. In our switching proposal, there are no self-proposals, so every self-transition comes from rejection.

Let us now substitute the proposal-and-acceptance form into detailed balance. We need pi i times q i j times alpha i j to equal pi j times q j i times alpha j i.

[Pause so the audience can connect each factor to a stage.]

The target and the proposal are given. The acceptance probabilities are what we can choose. They must correct any imbalance between the two proposed probability flows.

In our example, the forward and reverse proposals are equally likely. The target probabilities are unequal. We therefore need unequal acceptance probabilities to make the final weighted flows match.

## Slide 9 — We have the proposal. Now design the correction.

**Presenter A · 2:00 · 19:00–21:00**

Before we choose the acceptance rule, let us collect the pieces.

We have a target distribution pi. We have an easy-to-simulate proposal matrix Q. We know that the actual off-diagonal transitions will be proposal probability times acceptance probability. Rejections will keep the chain at its current state.

The remaining task is to select the acceptance probabilities so that the weighted probability flow in each direction matches. That will give us detailed balance and therefore stationarity.

There are also practical conditions to check, including whether the final chain can reach every relevant state. Detailed balance alone does not check that for us.

[Point along the construction checklist, then turn to Presenter B.]

We now have a proposal mechanism. The remaining question is: how should we choose the acceptance probabilities so that our target distribution is preserved?

My co-presenter will introduce the familiar acceptance rule, show why we do not need the normalizing constant, and complete the two-state construction.

## Slide 10 — Accept according to target mass and proposal flow.

**Presenter B · 3:00 · 21:00–24:00**

We will use the acceptance rule commonly called Metropolis–Hastings. First compute the ratio of pi j times q j i to pi i times q i j. Then take the minimum of one and that ratio.

If the ratio is at least one, accept the proposed move. If it is below one, accept with probability equal to the ratio. Operationally, we can draw a uniform random number between zero and one and accept when it is below the acceptance probability.

There are two pieces inside the ratio. The target ratio compares how much target probability belongs at the proposed and current states. The proposal ratio corrects for asymmetry in how readily we propose the forward and reverse moves.

[Point separately to the target ratio and proposal correction.]

If proposals in both directions are equally likely, the proposal terms cancel. We recover the simpler Metropolis rule based on the target ratio alone. With a symmetric proposal, moving toward a higher target probability is always accepted.

For a general proposal, that last statement does not hold on its own. We must consider the reverse-to-forward proposal ratio as well. This correction is an important part of Hastings's generalization.

For now, assume positive target probabilities and a pair of moves with positive proposal probabilities in both directions. If a proposed move has no possible reverse proposal, its Metropolis–Hastings acceptance probability is zero. The support of the proposal therefore matters for whether the resulting chain can explore the target.

## Slide 11 — A short correctness check: the same flow both ways.

**Presenter B · 2:30 · 24:00–26:30**

We can verify this acceptance rule with one short calculation.

Write the forward proposed flow as A, equal to pi i times q i j. Write the reverse proposed flow as B, equal to pi j times q j i. The acceptance probability in the forward direction is the minimum of one and B divided by A.

Multiplying by the proposed flow A gives the minimum of A and B. Doing the same calculation in the reverse direction gives the minimum of B and A. They are the same quantity.

[Pause after each side of the equality.]

So the accepted flow in each direction is the smaller of the two proposed flows. If one direction proposes more probability flow than the other, the acceptance mechanism reduces it enough to match.

This is a useful interpretation of rejection. Rejection adjusts the movement mechanism so that it respects the target probabilities. It can be necessary for correctness.

Since these accepted flows satisfy detailed balance, pi is stationary for the resulting transition matrix. That verifies the preservation property we were trying to build.

We will keep the derivation at this level today. The instructor's lecture will develop the detailed-balance derivation further. Our next step is to connect the familiar rule to Hastings's more general original formulation.

## Slide 12 — Hastings gives a family of acceptance rules.

**Presenter B · 2:00 · 26:30–28:30**

There is a small historical and mathematical point worth making. Detailed balance does not uniquely determine the acceptance rule.

In Equation 6, Hastings introduces a symmetric quantity s i j. The forward acceptance probability is s i j times B divided by A plus B. The reverse acceptance probability uses the same s i j, multiplied by A divided by A plus B.

When we multiply these probabilities by their proposed flows, both directions give s i j times A B divided by A plus B. That is why symmetry is useful.

However, symmetry is not the only requirement. We must choose the scale so that both acceptance probabilities are between zero and one. An arbitrary symmetric number will not necessarily work.

Different valid choices produce different acceptance rules. The familiar minimum rule is one choice. Barker's rule is another, obtained by setting the symmetric scale to one.

We have put the exact bound and comparison in the backup material. For the main presentation, the message is that Hastings provides a general construction, and Metropolis–Hastings is the particular choice we will now use to finish our example.

## Slide 13 — The normalizing constant disappears from the ratio.

**Presenter B · 2:30 · 28:30–31:00**

Suppose we only know the target through positive weights w i. The actual probabilities are w i divided by a common normalizing constant Z.

In a large problem, Z might require summing over an enormous state space or evaluating a difficult integral. Computing it can be the obstacle that prevents us from working with normalized probabilities directly.

Now substitute w j over Z and w i over Z into the acceptance ratio. The same factor appears in both numerator and denominator. It cancels, leaving w j times q j i divided by w i times q i j.

[Trace the two copies of Z, then pause at the final expression.]

The algorithm therefore needs relative target weights at the current and proposed states. It does not need the common normalizing constant. That is a major practical advantage emphasized in the paper's introduction.

Our two-state weights are three and one. We know their sum is four, but the acceptance calculation will use the weights directly. The normalized probabilities will only be needed when we verify our answer by hand.

Cancellation does not mean that every computational difficulty disappears. We still need to evaluate the relevant weights, simulate the proposal, and choose moves that explore the target effectively. The advantage is specifically that the common normalization calculation is unnecessary for this acceptance rule.

## Slide 14 — For our target, one direction needs more rejection.

**Presenter B · 2:30 · 31:00–33:30**

Let us complete the example. Our weights are three for state zero and one for state one. The proposal always switches states, so both off-diagonal proposal probabilities equal one.

Starting at zero and proposing one, the acceptance ratio is one divided by three. The minimum of one and one third is one third. So we accept this proposed move with probability one third.

Starting at one and proposing zero, the ratio is three. The minimum of one and three is one. So this proposed move is always accepted.

[Ask the audience to calculate the second probability before you read it aloud.]

Notice that the proposal mechanism is symmetric, but the acceptance mechanism is not. This is what we need: the chain should remain in state zero more often, because zero has more target probability.

Suppose we are currently at zero and the uniform random number is zero point seven. The move to one is rejected, so the next recorded state is zero again. If the uniform number were zero point two, the move would be accepted instead.

These are illustrative decisions, not results from a simulation. They show how to translate the acceptance probability into an actual random transition. We now have everything needed to construct the final matrix.

## Slide 15 — Construct P, then verify the distribution we wanted.

**Presenter B · 3:00 · 33:30–36:30**

The actual probability of moving from zero to one is the proposal probability, one, multiplied by the acceptance probability, one third. So p zero one is one third. The remaining probability, two thirds, is the probability of staying at zero.

From one, we propose zero and always accept. Therefore p one zero equals one, and p one one equals zero. These four values give the transition matrix on the slide.

First check that its entries are valid probabilities and that both rows sum to one.

Next check the probability flows under the target. Three quarters times one third is one quarter. One quarter times one is also one quarter. The two flows match.

Finally, multiply the target row vector by P. The probability of ending at zero is three quarters times two thirds, plus one quarter times one. This is one half plus one quarter, or three quarters. The probability of ending at one is three quarters times one third, plus one quarter times zero, or one quarter.

[Work through the two components slowly rather than reading only the final equality.]

The result is exactly the original target distribution. We have constructed a chain with the prescribed stationary distribution.

This is the full design sequence: choose the target, choose the proposal, calculate the acceptance probabilities, construct the actual transitions, and verify stationarity. The same logic applies beyond two states, even when writing down the full matrix is impractical.

## Slide 16 — Rejected proposals are part of the sample.

**Presenter B · 2:30 · 36:30–39:00**

Here is a question about implementation. Suppose we delete every rejected proposal and keep only observations produced by accepted moves. Will those recorded observations still have the target proportions?

[Wait for answers. Ask someone to describe the accepted moves in this particular example.]

Every accepted move switches states. So the retained observations would alternate between zero and one. Over a long record, the proportions would be one half and one half. That is the wrong target.

The repeated zeros in the full chain are essential. They are the time the chain spends at zero after refusing a proposed departure. Those repetitions help produce the desired three-to-one occupation ratio.

This is why the algorithm records the current state after every iteration, whether the proposal was accepted or rejected. Rejection does not mean that the iteration disappears.

There are specialized methods for working with accepted moves and appropriate weights, but simply deleting repeated states is not the algorithm we have constructed.

Also, a very high rejection rate can indicate an inefficient proposal. That is a separate issue from recording the chain correctly. We should investigate the proposal rather than erase the repetitions and thereby change the sampling procedure.

[Point back to the self-loop at zero.]

This loop is part of the mechanism that makes the stationary probabilities correct.

## Slide 17 — A small algorithm, with conditions we must check.

**Presenter B · 1:30 · 39:00–40:30**

The pseudocode now follows our construction directly. Start in a state with positive target weight. Propose a candidate, compute the acceptance probability, and draw a uniform random number. Update the current state if the move is accepted. Then record the current state, including after rejection.

For a proposal of the current state itself, set the acceptance probability to one. Accepting that proposal leaves the state unchanged.

There is one more structural check. Hastings explicitly says that irreducibility must be checked in each application. In a finite state space, we need a path of positive-probability transitions between every pair of relevant states.

Our example passes: zero can reach one, and one can reach zero. It also has a positive self-loop at zero. Together with irreducibility, that makes the chain aperiodic. The usual finite-state convergence of the distribution therefore applies as well as the long-run averaging result.

We should keep those statements separate. Stationarity is the preservation equation we verified; the additional properties address what happens when we run the chain from a chosen starting state. The instructor will explore those distinctions further.

## Slide 18 — Design the transitions. Preserve the target.

**Presenter B · 1:30 · 40:30–42:00**

Let us return to our original question. How can we construct a chain with a stationary distribution that we choose?

Start with a target and an easy proposal. Use an acceptance rule that balances the weighted flows. Record the state after every iteration. Then check both the preservation property and the chain's ability to move through the target's support.

The normalizing constant cancels because the acceptance rule uses a ratio of target probabilities. The resulting observations are generally dependent, so their Monte Carlo uncertainty needs appropriate treatment.

[If time permits, ask the audience to complete two short statements.]

If a proposal is rejected, we record what? The current state again.

And what equation did our construction guarantee? Pi P equals pi.

Our example shows the entire argument in a setting where we can calculate every entry. Hastings's contribution is the general construction that lets the same idea work with much more flexible proposals and much more complicated targets. Thank you.

## Slide 19 — An asymmetric proposal needs the correction factor.

**Backup · Presenter B · Use for questions**

Keep weights (3, 1), but use Q = [[1/2, 1/2], [1, 0]]. Then the ratio for 0 → 1 is (1 × 1)/(3 × 1/2) = 2/3; the ratio for 1 → 0 is 3/2, so acceptance is one. Actual movement probabilities remain 1/3 and one, giving the same P as before. Remaining at zero now includes a self-proposal as well as rejected departures. Omitting the proposal correction would give p01 = 1/6 and p10 = 1, whose stationary distribution is (6/7, 1/7), not our target.

## Slide 20 — What can the symmetric factor s actually be?

**Backup · Presenter B · Use for questions**

For A = π_i q_ij > 0 and B = π_j q_ji > 0, both acceptance probabilities are valid when 0 < s_ij ≤ (A+B)/max(A,B). With r = B/A, this upper bound is 1 + min(r, 1/r). Setting s = 1 gives Barker's acceptance B/(A+B) = r/(1+r). Taking the upper bound gives min(1,r), the Metropolis–Hastings choice. Symmetry ensures equal flows; the bound ensures valid probabilities. Zero-flow cases require separate support handling.

## Slide 21 — Stationarity, convergence and independence differ.

**Backup · Either presenter · Use for questions**

Stationarity means πP = π. Finite-state irreducibility gives a unique stationary distribution and convergence of occupation averages. Irreducibility plus aperiodicity gives convergence of the state distribution from arbitrary starting states. Starting with X_0 distributed as π makes every marginal stationary from the beginning, but does not generally make observations independent. These are separate claims. For example, stationarity concerns marginal distributions; independence would require the joint distribution at two times to factor into the product of those marginals. In our example, knowing the current state is one tells us the next state will be zero, so the observations are clearly dependent even when initialized at stationarity.

## Slide 22 — Dependence changes the uncertainty of an average.

**Backup · Either presenter · Use for questions**

For a stationary process Y_t = f(X_t) with variance σ², Var(mean Y) = σ²/N × [1 + 2 Σ_(k=1)^(N−1) (1−k/N)ρ_k]. Under summable correlations, the large-N factor approaches 1 + 2 Σ_(k≥1)ρ_k. Correlation can increase or decrease variance relative to independent observations. Our two-state chain has nontrivial eigenvalue −1/3; for the state indicator, ρ_k = (−1/3)^k. Avoid claiming that dependence always increases Monte Carlo error. Detailed variance estimation is outside today's main construction.

## Slide 23 — Sources and scope

**Backup · Either presenter · Reference material**

The primary source is Hastings (1970), *Biometrika*, 57(1), 97–109, DOI: 10.1093/biomet/57.1.97. The assigned scope is Sections 1, 2.1, and 2.2, on pages 97–101. The instructor's guide also points to *Pattern Recognition and Machine Learning*, Section 11.2, Murphy, Section 12.2, and Linderman's Markov Chain Monte Carlo notes. These are supplementary pointers supplied by the course; our central claims and construction follow Hastings. The two-state example and the asymmetric-proposal variation were designed for this presentation. Neither is presented as one of Hastings's original numerical examples. No empirical simulation result is claimed in the main slides.

## Rehearsal and timing

- Rehearse the handoff at exactly the end of slide 9, near minute 21.
- Practise saying “reverse proposal probability divided by forward proposal probability” while pointing to the correct terms.
- Use the pauses on slides 14–16 for audience calculation; do not rush the matrix multiplication.
- If the 40–45-minute allocation includes questions, target 37–39 minutes of prepared delivery. Shorten slides 2–3, 5–6, 9, and 12; keep the full worked example and rejection question.
- The backup slides are for questions, not part of the 42-minute main sequence.
- Page references: motivation and normalizing constants, pp. 97–98 (§1); Markov averages and dependence, pp. 98–99 (§2.1); detailed balance, proposal/acceptance construction, and general acceptance family, pp. 99–101 (§2.2).
