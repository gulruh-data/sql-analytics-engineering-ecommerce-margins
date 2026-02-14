/* PROJECT: E-commerce Financial Analytics Engineering
  GOAL: Creating reusable logic for Margin and Promo calculations 
  to ensure data consistency across the organization.
*/

-- 1. Function: Profit Margin Percentage
-- Calculates the profitability of each product with 1-decimal precision.
CREATE OR REPLACE FUNCTION `course17.margin_percent`(turnover FLOAT64, purchase_cost FLOAT64) AS (
  ROUND(SAFE_DIVIDE(turnover - purchase_cost, turnover) * 100, 1)
);

-- 2. Function: Promotion Percentage
-- Calculates the discount depth applied to each order.
CREATE OR REPLACE FUNCTION `course17.promo_percent`(turnover FLOAT64, turnover_before_promo FLOAT64) AS (
  CAST(ROUND(SAFE_DIVIDE(turnover_before_promo - turnover, turnover_before_promo) * 100) AS INT64)
);

-- 3. Executive Sales View
-- Combining all financial metrics into a single enriched sales table.
SELECT
      *,
      `course17.margin_percent`(turnover, purchase_cost) AS margin_percent,
      `course17.promo_percent`(turnover, turnover_before_promo) AS promo_percent
  FROM `course17.gwz_sales_17`
  ORDER BY date_date DESC;
