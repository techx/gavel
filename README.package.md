# Gavel Ranking

Gavel Ranking provides the online Crowd-BT model from
[Gavel](https://github.com/techx/gavel). It updates estimates of project quality
and judge reliability after each head-to-head vote. It can also estimate the
information gain of a proposed comparison. See
[Designing a Better Judging System](https://anishathalye.com/designing-a-better-judging-system/)
for the rationale behind pairwise judging.

The library requires Python 3.12 or later. Install it after the first PyPI release
with `python -m pip install gavel-ranking`. To try a checkout before release,
run `python -m pip install .` from its root.

```python
from gavel_ranking import crowd_bt

judge = (crowd_bt.ALPHA_PRIOR, crowd_bt.BETA_PRIOR)
project = (crowd_bt.MU_PRIOR, crowd_bt.SIGMA_SQ_PRIOR)

# Compare two projects with their current estimates.
gain = crowd_bt.expected_information_gain(*judge, *project, *project)

# Record a vote. Pass the winning project's estimates first.
alpha, beta, winner_mu, winner_var, loser_mu, loser_var = crowd_bt.update(
    *judge, *project, *project
)
```

Persist the returned judge and project estimates for the next vote. Higher
project means indicate better estimated quality. Replay historical votes in
chronological order when rebuilding state. The application supplies IDs,
persistence, and dispatch rules.

Gavel's original implementation is by Anish Athalye. This distribution is
licensed under the [GNU Affero General Public License v3](https://www.gnu.org/licenses/agpl-3.0.html).
