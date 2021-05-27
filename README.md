# Incremental Risk Charge (IRC) Introduction

The incremental risk charge (IRC) is a regulatory requirement from the Basel Committee in response to the financial crisis. It supplements existing Value-at-Risk (VaR) and captures the loss due to default and migration events at a 99.9% confidence level over a one-year capital horizon. 

The liquidity of a position is explicitly modeled in IRC through liquidity horizon and constant level of risk. The constant level of risk is a new concept in IRC. It assumes banks hold portfolio constant over a liquidity horizon. At the beginning of the next horizon, they rebalance any default, downgraded, or upgraded positions and roll over any matured trades. This presentation describes methodology and implementation details of IRC. 

Keywords:
Incremental risk charge, IRC, default risk, credit migration risk, constant level of risk, liquidity horizon, risk concentration.

	Market Risk Types
	General market risk
	Idiosyncratic or specific risk: equity specific risk and debt specific risk
	Even risk (e.g., default or migration): IRC

	IRC Definition
	The incremental risk charge (IRC) is a new regulatory requirement from the Basel Committee in response to the recent financial crisis.
	IRC supplements existing Value-at-Risk (VaR) and captures the loss due to default and migration events at a 99.9% confidence level over a one-year capital horizon.

	IRC Scope
	Debt instruments are subject to IRC.
	Credit products, including structured credit

	IRC Main Features
	Liquidity: The liquidity of position is explicitly modeled in IRC through liquidity horizon and constant level of risk.
	Constant level of risk: assumption
	Hold portfolio constant over liquidity horizon
	Rebalance any default, downgraded, or upgraded positions at the beginning of each liquidity horizon 
	Roll over any matured positions at the beginning of each horizon
	Default and migration: banks need to simulate default and migration for one-year horizon.
	Concentration: it measures the degree of a portfolio diversification. For example, if a significant number of issuers belong to a certain category, the portfolio is a concentrated one.

	Simulate default and migration
	Default and credit migration is commonly modeled by an asset model.
 	
where
	 		The independent standard normally random variables
	 		The systematic risk
	 		The idiosyncratic risk for issuer/obligor i
 	The weighted correlation that systematic risk factor affects issuer/obligor i. 
 	The normalized asset return or creditworthiness indicator for issuer/obligor i
	Determination of default and credit migration
	Given historical default and transition probabilities (also called default transition matrix), the thresholds of default and credit migration can be computed.
	For example, we can compute various rating thresholds for a BBB issuer as
Z_BBB^D , Z_BBB^CCC, Z_BBB^B, Z_BBB^BB, z_BBB^BBB, Z_BBB^A, Z_BBB^AA, Z_BBB^AAA
	If the simulated normalized asset value z_i is between Z_BBB^A and Z_BBB^AA, it means the issuer is migrated from BBB to AA, verse vice.
	Similarly if the simulated asset value  z_i is smaller than Z_BBB^D , the issuer defaults

	Constant level of risk
	The constant level of risk reflects recognition by regulators that securities/derivatives held in the trading book are generally much more liquid than those in the banking book.
	We interpret constant level of risk as constant loss distribution, i.e.,
	The same loss distribution over each liquidity horizon
	The same rating over each liquidity horizon
	The same risk metrics over each liquidity horizon
	For example, the liquidity horizon for a portfolio is 3 months. That means the bank holds its portfolio components constant for 3 months and then rebalances it by replacing any default or downgraded or upgraded positions so that the portfolio is returned to the initial level of risk.
	The processes are repeated four times to arrive at 1-year shown as
 
	In Monte Carlo context, this can be modeled by drawing 4 times from the single period loss distribution measured over the liquidity horizon.
	The advantages of the same loss distribution assumption
	Avoid the complexity of rebalancing and roll-over
	Reduce computation significantly
	Implementation
	Find all debt and credit deals.
	Banks can assign a liquidity horizon to each deal under conservative assumption. The liquidity horizon has a floor of 3 months
	Divide deals into portfolios based on liquidity horizons. 
	Assuming that a portfolio has 3-months liquidity horizon, compute 3-month loss distribution based on default and migration simulation.
	Simulate default and migration at 3 months
	If default: 	〖DefaultLoss〗_(i,3m)=〖Exposure〗_(i,3m)*〖LGD〗_i	
	If rating change: 	〖MigrationLoss〗_(i,3m)=〖MTM〗_(i,3m,newRating)-〖MTM〗_(i,0,oldRating)
	Total loss: 	〖loss〗_3m=∑_i▒〖〖DefaultLoss〗_(i,3m)+∑_j▒〖MigrationLoss〗_(j,3m) 〗
	Repeat for all scenarios to generate 3 month loss distribution
	Based on the constant level of risk assumption, the 3-6 months, 6-9 months and 9-12 months loss distributions are just the copy of 0-3 months lost distribution.
	The 1-year loss distribution is the convolution of 4 copies of the first 3-month loss distribution.
	IRC = 99.9% quantile of the 1-year loss distribution


References:

https://finpricing.com/lib/FxVolIntroduction.html

https://bitbucket.org/cfrm17/irc/downloads/irc-9.pdf

