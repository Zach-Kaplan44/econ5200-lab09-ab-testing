# econ5200-lab09-ab-testing
# Diagnosing Experimental Failures in A/B Testing

## Objective
I tested how common problems in A/B testing, including buggy variance reduction, sample ratio mismatch, interference between groups, weak evidence, and peeking, can lead to wrong conclusions, and how to catch them.

## Methodology
- Reviewed an analyst's CUPED (variance reduction) code, found three bugs, fixed them, and compared the results before and after.
- Ran chi-squared sample ratio mismatch tests on two experiments of very different sizes.
- Simulated a marketplace where buyers in the discount group take inventory away from the control group, then compared the A/B test's naive ATE with the effect of rolling the discount out to everyone.
- Ran a Bayesian A/B test, calculating P(B > A) and the expected loss of shipping B, applied a ship rule, and checked the result with a permutation test.
- Wrote my own function for the Bayesian test and ran it on a test with four times as many users.
- Simulated 1,000 experiments with no true effect, checking for significance at 50 points in each, and compared the false positive rate to a single test at the end.

## Key Findings
- **CUPED:** The analyst reported a 0.0% variance reduction. After I fixed the three bugs, the reduction was 82.7%, and the standard error dropped from 0.1539 to 0.0641.
- **Sample ratio mismatch:** A 0.60 pp imbalance among 100,000 users gave a chi-squared of 14.40. A 0.05 pp imbalance among 20 million users gave 20.00. A very small imbalance can still signal a real problem when the sample is large.
- **Marketplace interference:** The naive ATE was 0.1861. The effect of rolling the discount out to everyone was 0.1428. The A/B test overstated the effect by 23.3% of the naive estimate, because the treatment group was partly taking sales from the control group, not creating new ones.
- **Bayesian test:** P(B > A) was 0.8866 and the expected loss of shipping B was 0.000473, so the ship rule said not to ship B. A permutation test agreed that the evidence was weak (p = 0.2302).
- **More users:** With my own function on a test with four times the users, P(B > A) rose to 0.9951.
- **Peeking:** When I checked at 50 points, 32.4% of experiments with no effect crossed p < 0.05 at least once. With a single test at the end, the rate was 5.3%. Checking results repeatedly and stopping at the first significant result greatly inflates false positives.
