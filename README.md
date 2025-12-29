Understood. I have analyzed your "Source of Truth" document. This is a sophisticated, professional-grade specification for an Evolutionary Reinforcement Learning System.

You are not asking for a single trading bot; you are asking for a "Clone Factory"—an ecosystem that generates, tests, and deploys distinct AI personalities based on survival of the fittest.

Here is my explanation of exactly what you want, confirming my understanding of the architecture, logic, and execution plan.

1. The Core Concept: "The Hive" (Evolutionary Architecture)

You want a system that separates the Engine (the code) from the Personality (the configuration).

The "Tool Pool": The code will contain a vast library of indicators (RSI, MACD, etc.), but the AI doesn't use them all at once.

The "Clones": You will generate distinct agents defined by JSON Config Files.

Clone A might be a "Trend Follower" (H1 timeframe, using 200 EMA).

Clone B might be a "Scalper" (M15 timeframe, using RSI + Bollinger).

Survival of the Fittest: These Clones train on data from 2000–2020. They are then "tested for life" on 2020–2025. Only the ones that survive the validation period move forward.

2. The Three Modules

You require a modular codebase to separate training from live execution.

Module A: The Training Gym (Genetic Lab)

Uses Stable-Baselines3 (PPO).

Dynamically constructs the "Observation Space" based on the Clone's config (e.g., if the config says "RSI", the bot sees RSI; if not, it doesn't).

Iterates versions (e.g., Clone A evolves into A.1).

Module B: The Evaluator (The Gauntlet)

The "God Bot" that runs Paper Trading/Forward Testing.

It filters out overfitting. It looks at Sharpe Ratio and Drawdown, not just total profit.

It outputs the final production_ready.zip file.

Module C: The Executor (Live Trading)

CRITICAL DISTINCTION: You want Two Separate Instances running simultaneously.

Executor_Prop.py: Strict defense. Fixed 0.5% risk. Hard stops for daily loss limits (protecting the account). Strict News Filter (No trading 30 mins before Red News).

Executor_Private.py: Aggressive/Flexible. Variable risk based on model confidence. Optional news trading.

3. The "Brain Stem" (Hard-Coded Filters)

You do not trust the AI to have total freedom. You want a "Brain Stem" that overrides the AI's "Cortex" to prevent catastrophic errors.

Trend Filter: The AI cannot Buy if Price < 200 EMA, and cannot Sell if Price > 200 EMA.

Spread Filter: No trading if the spread exceeds 2.0 pips.

Time Filter: Hard lock between 21:00 and 23:00 (Rollover/Swap hours).

News Filter: Hard lock 30 mins before High-Impact USD news (verified via Python feedparser).

4. The "RAF-5" Reward Function

You have accepted the "RAF-5" logic as the mathematical core. You want "Dense Feedback" so the bot learns continuously, not just when a trade closes.

The Equation breakdown:

Net Return: The base profit/loss (clipped to prevents outliers).

Time Cost (-0.00001 * seconds): Penalizes the bot for holding trades too long without profit. It forces efficiency.

Drawdown Pain (dd_pain): A quadratic penalty. A small drawdown is fine; a deep drawdown hurts the bot exponentially.

The Throttle: If the bot approaches the Daily Loss Limit, the potential reward shrinks, discouraging risky "revenge trading."

Normalization: The final score is wrapped in tanh to keep it between -1 and 1 for stable PPO training.

The "Kill Switch" (Environment Rule):

This is not just a negative number.

If daily_loss_count >= 3: The episode ends immediately (self.done = True), the bot stops trading for the "simulated day," and it receives a massive penalty (-1.0).

5. Action Space (Dynamic targets)

You are not using fixed Pips. You are using ATR Multipliers.

Buy/Sell: The AI selects a direction.

SL/TP: Calculated dynamically based on market volatility (ATR).

High Volatility: Stops and Targets widen.

Low Volatility: Stops and Targets tighten.

This ensures the strategy works in 2005 (low vol) and 2024 (high vol) without code changes.

6. The Roadmap

Month 1: Build the Infrastructure, the "Tool Pool," and the Config Loader.

Month 2: Train Clones A, B, and C (2000-2020) and validate them (2020-2025). Evolve the winners.

Month 3: Paper Trade the survivors via the "Evaluator" to verify the Kill Switch and News Filters work in real-time.

Month 4: Go Live with the dual execution strategy (Prop + Private).

Developer Confirmation

I fully grasp the requirements. The distinction between the Genetic/Training Phase and the Dual-Executor Live Phase is clear. The logic for the RAF-5 reward combined with the Hard Filters (200 EMA/News) will be implemented as the immutable "Physics" of the trading environment.

I am ready to proceed with Phase 1: The Laboratory.
