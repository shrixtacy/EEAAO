# Python Data Analytics - Business Intelligence Path

Transform raw data into actionable business insights. This isn't about becoming a statistician - it's about answering business questions with data and communicating findings that drive decisions.

## Reality Check First

**What Data Analytics Actually Is:**
- 70% data cleaning and preparation
- 20% analysis and visualization
- 10% the "advanced analytics" you see in tutorials

**What You'll Actually Build:**
- Business dashboards and reports
- Sales and marketing analytics
- Customer behavior analysis
- Financial performance tracking
- Operational efficiency metrics
- Predictive business models

**What You Won't Become:**
- A data scientist (different focus and depth)
- Someone who ignores business context
- A replacement for domain expertise

## Prerequisites

**From Python Fundamentals:**
- Comfortable with data structures (lists, dictionaries)
- Understanding of loops, functions, and file operations
- Experience with libraries and package management

**Additional Skills:**
- **Excel Proficiency**: Understanding of spreadsheets and basic formulas
- **Business Acumen**: Understanding of business metrics and KPIs
- **SQL Basics**: Querying databases and joining tables
- **Statistical Thinking**: Basic understanding of averages, trends, correlations

## Phase 1: Data Manipulation and Analysis

### Pandas - Your Data Analysis Powerhouse

**Why Pandas**: Industry standard for data manipulation in Python

```python
import pandas as pd
import numpy as np
from datetime import datetime, timedelta
import matplotlib.pyplot as plt
import seaborn as sns

class SalesAnalyzer:
    def __init__(self, data_source=None):
        if data_source:
            self.df = pd.read_csv(data_source)
        else:
            # Generate realistic sample sales data
            self.df = self.generate_sample_data()
        
        # Clean and prepare data
        self.prepare_data()
    
    def generate_sample_data(self, n_records=10000):
        """Generate realistic sales data for analysis"""
        np.random.seed(42)
        
        # Date range for the last 2 years
        start_date = datetime.now() - timedelta(days=730)
        dates = pd.date_range(start_date, periods=n_records, freq='H')
        
        # Product categories and their characteristics
        products = {
            'Electronics': {'price_range': (50, 2000), 'seasonality': 1.2},
            'Clothing': {'price_range': (20, 300), 'seasonality': 1.5},
            'Home & Garden': {'price_range': (15, 500), 'seasonality': 0.8},
            'Books': {'price_range': (10, 50), 'seasonality': 1.0},
            'Sports': {'price_range': (25, 800), 'seasonality': 1.3}
        }
        
        # Customer segments
        segments = ['Premium', 'Standard', 'Budget']
        segment_multipliers = {'Premium': 2.5, 'Standard': 1.0, 'Budget': 0.6}
        
        # Sales regions
        regions = ['North', 'South', 'East', 'West', 'Central']
        
        # Generate data
        data = []
        for i, date in enumerate(dates):
            # Add seasonality (higher sales in Nov-Dec)
            seasonal_factor = 1.0
            if date.month in [11, 12]:
                seasonal_factor = 1.4
            elif date.month in [1, 2]:
                seasonal_factor = 0.7
            
            # Add day-of-week effect (higher on weekends)
            dow_factor = 1.2 if date.weekday() >= 5 else 1.0
            
            # Add hour-of-day effect (higher during business hours)
            hour_factor = 1.0
            if 9 <= date.hour <= 17:
                hour_factor = 1.3
            elif 18 <= date.hour <= 21:
                hour_factor = 1.1
            
            # Select random product and characteristics
            category = np.random.choice(list(products.keys()))
            product_info = products[category]
            
            # Generate price with some randomness
            base_price = np.random.uniform(*product_info['price_range'])
            price = base_price * seasonal_factor * product_info['seasonality']
            
            # Generate quantity (usually 1-3 items)
            quantity = np.random.choice([1, 1, 1, 2, 2, 3], p=[0.5, 0.2, 0.1, 0.1, 0.05, 0.05])
            
            # Select customer segment and region
            segment = np.random.choice(segments, p=[0.2, 0.6, 0.2])
            region = np.random.choice(regions)
            
            # Apply segment multiplier
            final_price = price * segment_multipliers[segment]
            
            # Calculate total
            total = final_price * quantity
            
            # Add some randomness to make it realistic
            total *= np.random.uniform(0.9, 1.1)
            
            data.append({
                'date': date,
                'category': category,
                'price': round(final_price, 2),
                'quantity': quantity,
                'total': round(total, 2),
                'customer_segment': segment,
                'region': region,
                'day_of_week': date.strftime('%A'),
                'month': date.strftime('%B'),
                'hour': date.hour
            })
        
        return pd.DataFrame(data)
    
    def prepare_data(self):
        """Clean and prepare data for analysis"""
        # Convert date column to datetime
        self.df['date'] = pd.to_datetime(self.df['date'])
        
        # Create additional time-based columns
        self.df['year'] = self.df['date'].dt.year
        self.df['month_num'] = self.df['date'].dt.month
        self.df['week'] = self.df['date'].dt.isocalendar().week
        self.df['quarter'] = self.df['date'].dt.quarter
        
        # Create profit margin (simplified)
        self.df['profit_margin'] = self.df['total'] * 0.3  # Assume 30% margin
        
        # Sort by date
        self.df = self.df.sort_values('date').reset_index(drop=True)
        
        print(f"Data prepared: {len(self.df)} records from {self.df['date'].min()} to {self.df['date'].max()}")
    
    def get_basic_stats(self):
        """Get basic statistics about the sales data"""
        stats = {
            'total_revenue': self.df['total'].sum(),
            'total_transactions': len(self.df),
            'average_order_value': self.df['total'].mean(),
            'total_profit': self.df['profit_margin'].sum(),
            'date_range': {
                'start': self.df['date'].min(),
                'end': self.df['date'].max()
            },
            'top_categories': self.df.groupby('category')['total'].sum().sort_values(ascending=False),
            'top_regions': self.df.groupby('region')['total'].sum().sort_values(ascending=False)
        }
        
        return stats
    
    def analyze_trends(self):
        """Analyze sales trends over time"""
        # Monthly trends
        monthly_sales = self.df.groupby(['year', 'month_num']).agg({
            'total': 'sum',
            'quantity': 'sum',
            'date': 'count'  # Number of transactions
        }).rename(columns={'date': 'transactions'})
        
        monthly_sales['avg_order_value'] = monthly_sales['total'] / monthly_sales['transactions']
        
        # Weekly trends
        weekly_sales = self.df.groupby('day_of_week').agg({
            'total': ['sum', 'mean', 'count']
        }).round(2)
        
        # Hourly trends
        hourly_sales = self.df.groupby('hour').agg({
            'total': ['sum', 'mean', 'count']
        }).round(2)
        
        return {
            'monthly': monthly_sales,
            'weekly': weekly_sales,
            'hourly': hourly_sales
        }
    
    def customer_segmentation_analysis(self):
        """Analyze customer segments"""
        segment_analysis = self.df.groupby('customer_segment').agg({
            'total': ['sum', 'mean', 'count'],
            'quantity': 'sum',
            'profit_margin': 'sum'
        }).round(2)
        
        # Calculate segment percentages
        total_revenue = self.df['total'].sum()
        segment_analysis[('percentage', 'revenue')] = (
            segment_analysis[('total', 'sum')] / total_revenue * 100
        ).round(1)
        
        return segment_analysis
    
    def product_performance_analysis(self):
        """Analyze product category performance"""
        category_analysis = self.df.groupby('category').agg({
            'total': ['sum', 'mean', 'count'],
            'quantity': 'sum',
            'profit_margin': 'sum',
            'price': 'mean'
        }).round(2)
        
        # Calculate growth rates (comparing first and second half of data)
        midpoint = len(self.df) // 2
        first_half = self.df.iloc[:midpoint]
        second_half = self.df.iloc[midpoint:]
        
        first_half_sales = first_half.groupby('category')['total'].sum()
        second_half_sales = second_half.groupby('category')['total'].sum()
        
        growth_rates = ((second_half_sales - first_half_sales) / first_half_sales * 100).round(1)
        category_analysis[('growth_rate', 'percent')] = growth_rates
        
        return category_analysis
    
    def regional_analysis(self):
        """Analyze regional performance"""
        regional_analysis = self.df.groupby('region').agg({
            'total': ['sum', 'mean', 'count'],
            'profit_margin': 'sum'
        }).round(2)
        
        # Calculate market share
        total_revenue = self.df['total'].sum()
        regional_analysis[('market_share', 'percent')] = (
            regional_analysis[('total', 'sum')] / total_revenue * 100
        ).round(1)
        
        return regional_analysis
    
    def seasonal_analysis(self):
        """Analyze seasonal patterns"""
        # Monthly seasonality
        monthly_seasonality = self.df.groupby('month').agg({
            'total': ['sum', 'mean'],
            'quantity': 'sum'
        }).round(2)
        
        # Quarterly analysis
        quarterly_analysis = self.df.groupby('quarter').agg({
            'total': ['sum', 'mean'],
            'profit_margin': 'sum'
        }).round(2)
        
        return {
            'monthly': monthly_seasonality,
            'quarterly': quarterly_analysis
        }
    
    def find_top_performers(self, metric='total', top_n=10):
        """Find top performing periods/segments"""
        # Top days
        daily_sales = self.df.groupby(self.df['date'].dt.date)[metric].sum().sort_values(ascending=False)
        
        # Top weeks
        weekly_sales = self.df.groupby([self.df['date'].dt.year, self.df['date'].dt.isocalendar().week])[metric].sum().sort_values(ascending=False)
        
        return {
            'top_days': daily_sales.head(top_n),
            'top_weeks': weekly_sales.head(top_n)
        }
    
    def generate_insights(self):
        """Generate business insights from the analysis"""
        insights = []
        
        # Basic stats
        stats = self.get_basic_stats()
        insights.append(f"Total revenue: ${stats['total_revenue']:,.2f} from {stats['total_transactions']:,} transactions")
        insights.append(f"Average order value: ${stats['average_order_value']:.2f}")
        
        # Top category
        top_category = stats['top_categories'].index[0]
        top_category_revenue = stats['top_categories'].iloc[0]
        insights.append(f"Top category: {top_category} (${top_category_revenue:,.2f})")
        
        # Customer segments
        segment_analysis = self.customer_segmentation_analysis()
        premium_revenue_pct = segment_analysis.loc['Premium', ('percentage', 'revenue')]
        insights.append(f"Premium customers generate {premium_revenue_pct}% of revenue")
        
        # Seasonal patterns
        seasonal = self.seasonal_analysis()
        best_quarter = seasonal['quarterly'][('total', 'sum')].idxmax()
        insights.append(f"Q{best_quarter} is the strongest quarter")
        
        # Regional performance
        regional = self.regional_analysis()
        top_region = regional[('total', 'sum')].idxmax()
        top_region_share = regional.loc[top_region, ('market_share', 'percent')]
        insights.append(f"Top region: {top_region} ({top_region_share}% market share)")
        
        return insights

# Advanced Analytics Functions
class AdvancedAnalytics:
    def __init__(self, sales_analyzer):
        self.analyzer = sales_analyzer
        self.df = sales_analyzer.df
    
    def cohort_analysis(self):
        """Perform cohort analysis to understand customer retention"""
        # Simplified cohort analysis based on customer segments and time periods
        # In real scenario, you'd have customer IDs
        
        # Group by month and segment
        cohort_data = self.df.groupby([
            self.df['date'].dt.to_period('M'),
            'customer_segment'
        ]).agg({
            'total': 'sum',
            'date': 'count'
        }).rename(columns={'date': 'transactions'})
        
        return cohort_data
    
    def market_basket_analysis(self):
        """Analyze which categories are often bought together"""
        # Simplified market basket analysis
        # Group transactions by day and customer segment (proxy for individual customers)
        daily_purchases = self.df.groupby([
            self.df['date'].dt.date,
            'customer_segment'
        ])['category'].apply(list).reset_index()
        
        # Find category combinations
        from itertools import combinations
        category_pairs = {}
        
        for categories in daily_purchases['category']:
            if len(categories) > 1:
                for pair in combinations(set(categories), 2):
                    pair_key = tuple(sorted(pair))
                    category_pairs[pair_key] = category_pairs.get(pair_key, 0) + 1
        
        # Sort by frequency
        sorted_pairs = sorted(category_pairs.items(), key=lambda x: x[1], reverse=True)
        
        return sorted_pairs[:10]  # Top 10 pairs
    
    def price_elasticity_analysis(self):
        """Analyze price sensitivity by category"""
        elasticity_data = []
        
        for category in self.df['category'].unique():
            category_data = self.df[self.df['category'] == category].copy()
            
            # Create price bins
            category_data['price_bin'] = pd.cut(category_data['price'], bins=5, labels=['Low', 'Med-Low', 'Medium', 'Med-High', 'High'])
            
            # Analyze quantity by price bin
            price_quantity = category_data.groupby('price_bin').agg({
                'quantity': 'mean',
                'price': 'mean',
                'total': 'sum'
            }).round(2)
            
            elasticity_data.append({
                'category': category,
                'price_sensitivity': price_quantity
            })
        
        return elasticity_data
    
    def forecast_sales(self, periods=30):
        """Simple sales forecasting using moving averages"""
        # Daily sales
        daily_sales = self.df.groupby(self.df['date'].dt.date)['total'].sum()
        
        # Calculate moving averages
        ma_7 = daily_sales.rolling(window=7).mean()
        ma_30 = daily_sales.rolling(window=30).mean()
        
        # Simple forecast using trend
        recent_trend = ma_7.tail(7).mean()
        last_date = daily_sales.index[-1]
        
        forecast_dates = pd.date_range(start=last_date + timedelta(days=1), periods=periods)
        forecast_values = [recent_trend] * periods
        
        forecast_df = pd.DataFrame({
            'date': forecast_dates,
            'forecasted_sales': forecast_values
        })
        
        return {
            'historical': daily_sales,
            'moving_averages': {'7_day': ma_7, '30_day': ma_30},
            'forecast': forecast_df
        }

# Usage example
def perform_comprehensive_analysis():
    """Perform comprehensive sales analysis"""
    print("Initializing Sales Analyzer...")
    analyzer = SalesAnalyzer()
    
    print("\n=== BASIC STATISTICS ===")
    stats = analyzer.get_basic_stats()
    for key, value in stats.items():
        if key not in ['top_categories', 'top_regions', 'date_range']:
            print(f"{key.replace('_', ' ').title()}: {value}")
    
    print(f"\nDate Range: {stats['date_range']['start'].date()} to {stats['date_range']['end'].date()}")
    
    print("\n=== TOP CATEGORIES ===")
    for category, revenue in stats['top_categories'].head().items():
        print(f"{category}: ${revenue:,.2f}")
    
    print("\n=== CUSTOMER SEGMENT ANALYSIS ===")
    segment_analysis = analyzer.customer_segmentation_analysis()
    print(segment_analysis)
    
    print("\n=== PRODUCT PERFORMANCE ===")
    product_analysis = analyzer.product_performance_analysis()
    print(product_analysis)
    
    print("\n=== REGIONAL ANALYSIS ===")
    regional_analysis = analyzer.regional_analysis()
    print(regional_analysis)
    
    print("\n=== BUSINESS INSIGHTS ===")
    insights = analyzer.generate_insights()
    for i, insight in enumerate(insights, 1):
        print(f"{i}. {insight}")
    
    print("\n=== ADVANCED ANALYTICS ===")
    advanced = AdvancedAnalytics(analyzer)
    
    # Market basket analysis
    basket_analysis = advanced.market_basket_analysis()
    print("\nTop Category Combinations:")
    for pair, count in basket_analysis[:5]:
        print(f"  {pair[0]} + {pair[1]}: {count} times")
    
    # Sales forecast
    forecast = advanced.forecast_sales(periods=7)
    print(f"\nNext 7 days average forecasted sales: ${forecast['forecast']['forecasted_sales'].mean():,.2f}/day")
    
    return analyzer

if __name__ == "__main__":
    # Run comprehensive analysis
    analyzer = perform_comprehensive_analysis()
```

### Data Visualization with Matplotlib and Seaborn

**Real-World Application**: Creating compelling business visualizations

```python
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
import plotly.graph_objects as go
from plotly.subplots import make_subplots
import pandas as pd
import numpy as np

class BusinessDashboard:
    def __init__(self, sales_analyzer):
        self.analyzer = sales_analyzer
        self.df = sales_analyzer.df
        
        # Set style for better-looking plots
        plt.style.use('seaborn-v0_8')
        sns.set_palette("husl")
        
        # Create figure directory
        import os
        os.makedirs('dashboard_plots', exist_ok=True)
    
    def create_revenue_dashboard(self):
        """Create comprehensive revenue dashboard"""
        fig, axes = plt.subplots(2, 3, figsize=(20, 12))
        fig.suptitle('Sales Revenue Dashboard', fontsize=16, fontweight='bold')
        
        # 1. Monthly Revenue Trend
        monthly_revenue = self.df.groupby([self.df['date'].dt.year, self.df['date'].dt.month])['total'].sum()
        monthly_revenue.index = pd.to_datetime([f'{year}-{month:02d}-01' for year, month in monthly_revenue.index])
        
        axes[0, 0].plot(monthly_revenue.index, monthly_revenue.values, marker='o', linewidth=2, markersize=6)
        axes[0, 0].set_title('Monthly Revenue Trend', fontweight='bold')
        axes[0, 0].set_xlabel('Month')
        axes[0, 0].set_ylabel('Revenue ($)')
        axes[0, 0].tick_params(axis='x', rotation=45)
        axes[0, 0].grid(True, alpha=0.3)
        
        # Format y-axis to show currency
        axes[0, 0].yaxis.set_major_formatter(plt.FuncFormatter(lambda x, p: f'${x/1000:.0f}K'))
        
        # 2. Category Performance
        category_revenue = self.df.groupby('category')['total'].sum().sort_values(ascending=True)
        bars = axes[0, 1].barh(category_revenue.index, category_revenue.values)
        axes[0, 1].set_title('Revenue by Category', fontweight='bold')
        axes[0, 1].set_xlabel('Revenue ($)')
        
        # Add value labels on bars
        for bar in bars:
            width = bar.get_width()
            axes[0, 1].text(width, bar.get_y() + bar.get_height()/2, 
                           f'${width/1000:.0f}K', ha='left', va='center')
        
        # 3. Regional Distribution (Pie Chart)
        regional_revenue = self.df.groupby('region')['total'].sum()
        wedges, texts, autotexts = axes[0, 2].pie(regional_revenue.values, labels=regional_revenue.index, 
                                                 autopct='%1.1f%%', startangle=90)
        axes[0, 2].set_title('Revenue Distribution by Region', fontweight='bold')
        
        # 4. Customer Segment Analysis
        segment_data = self.df.groupby('customer_segment').agg({
            'total': 'sum',
            'date': 'count'
        }).rename(columns={'date': 'transactions'})
        
        x = np.arange(len(segment_data.index))
        width = 0.35
        
        bars1 = axes[1, 0].bar(x - width/2, segment_data['total']/1000, width, label='Revenue ($K)')
        bars2 = axes[1, 0].bar(x + width/2, segment_data['transactions']/10, width, label='Transactions (×10)')
        
        axes[1, 0].set_title('Customer Segment Performance', fontweight='bold')
        axes[1, 0].set_xlabel('Customer Segment')
        axes[1, 0].set_xticks(x)
        axes[1, 0].set_xticklabels(segment_data.index)
        axes[1, 0].legend()
        
        # 5. Daily Sales Pattern
        daily_pattern = self.df.groupby('day_of_week')['total'].mean()
        day_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
        daily_pattern = daily_pattern.reindex(day_order)
        
        bars = axes[1, 1].bar(daily_pattern.index, daily_pattern.values, 
                             color=['#ff9999' if day in ['Saturday', 'Sunday'] else '#66b3ff' for day in daily_pattern.index])
        axes[1, 1].set_title('Average Daily Sales Pattern', fontweight='bold')
        axes[1, 1].set_xlabel('Day of Week')
        axes[1, 1].set_ylabel('Average Revenue ($)')
        axes[1, 1].tick_params(axis='x', rotation=45)
        
        # 6. Hourly Sales Heatmap
        hourly_data = self.df.groupby(['day_of_week', 'hour'])['total'].sum().unstack(fill_value=0)
        hourly_data = hourly_data.reindex(day_order)
        
        im = axes[1, 2].imshow(hourly_data.values, cmap='YlOrRd', aspect='auto')
        axes[1, 2].set_title('Sales Heatmap (Day vs Hour)', fontweight='bold')
        axes[1, 2].set_xlabel('Hour of Day')
        axes[1, 2].set_ylabel('Day of Week')
        axes[1, 2].set_yticks(range(len(day_order)))
        axes[1, 2].set_yticklabels(day_order)
        axes[1, 2].set_xticks(range(0, 24, 4))
        
        # Add colorbar
        plt.colorbar(im, ax=axes[1, 2], label='Revenue ($)')
        
        plt.tight_layout()
        plt.savefig('dashboard_plots/revenue_dashboard.png', dpi=300, bbox_inches='tight')
        plt.show()
    
    def create_trend_analysis_plots(self):
        """Create detailed trend analysis visualizations"""
        fig, axes = plt.subplots(2, 2, figsize=(16, 12))
        fig.suptitle('Trend Analysis Dashboard', fontsize=16, fontweight='bold')
        
        # 1. Revenue vs Profit Trend
        monthly_data = self.df.groupby([self.df['date'].dt.year, self.df['date'].dt.month]).agg({
            'total': 'sum',
            'profit_margin': 'sum'
        })
        monthly_data.index = pd.to_datetime([f'{year}-{month:02d}-01' for year, month in monthly_data.index])
        
        ax1 = axes[0, 0]
        ax2 = ax1.twinx()
        
        line1 = ax1.plot(monthly_data.index, monthly_data['total'], 'b-', marker='o', label='Revenue')
        line2 = ax2.plot(monthly_data.index, monthly_data['profit_margin'], 'r-', marker='s', label='Profit')
        
        ax1.set_xlabel('Month')
        ax1.set_ylabel('Revenue ($)', color='b')
        ax2.set_ylabel('Profit ($)', color='r')
        ax1.set_title('Revenue vs Profit Trend', fontweight='bold')
        
        # Combine legends
        lines = line1 + line2
        labels = [l.get_label() for l in lines]
        ax1.legend(lines, labels, loc='upper left')
        
        # 2. Category Growth Rates
        # Calculate month-over-month growth for each category
        category_monthly = self.df.groupby(['category', self.df['date'].dt.to_period('M')])['total'].sum().unstack(fill_value=0)
        growth_rates = category_monthly.pct_change(axis=1).iloc[:, -6:]  # Last 6 months
        
        sns.heatmap(growth_rates, annot=True, fmt='.1%', cmap='RdYlGn', center=0, ax=axes[0, 1])
        axes[0, 1].set_title('Category Growth Rates (Last 6 Months)', fontweight='bold')
        axes[0, 1].set_xlabel('Month')
        
        # 3. Seasonal Decomposition (simplified)
        daily_sales = self.df.groupby(self.df['date'].dt.date)['total'].sum()
        
        # Calculate 7-day and 30-day moving averages
        ma_7 = daily_sales.rolling(window=7).mean()
        ma_30 = daily_sales.rolling(window=30).mean()
        
        axes[1, 0].plot(daily_sales.index, daily_sales.values, alpha=0.3, label='Daily Sales')
        axes[1, 0].plot(ma_7.index, ma_7.values, label='7-day MA', linewidth=2)
        axes[1, 0].plot(ma_30.index, ma_30.values, label='30-day MA', linewidth=2)
        
        axes[1, 0].set_title('Sales Trend with Moving Averages', fontweight='bold')
        axes[1, 0].set_xlabel('Date')
        axes[1, 0].set_ylabel('Sales ($)')
        axes[1, 0].legend()
        axes[1, 0].tick_params(axis='x', rotation=45)
        
        # 4. Price vs Quantity Analysis
        # Create scatter plot of price vs quantity by category
        for category in self.df['category'].unique():
            category_data = self.df[self.df['category'] == category]
            axes[1, 1].scatter(category_data['price'], category_data['quantity'], 
                             label=category, alpha=0.6, s=30)
        
        axes[1, 1].set_title('Price vs Quantity by Category', fontweight='bold')
        axes[1, 1].set_xlabel('Price ($)')
        axes[1, 1].set_ylabel('Quantity')
        axes[1, 1].legend()
        axes[1, 1].grid(True, alpha=0.3)
        
        plt.tight_layout()
        plt.savefig('dashboard_plots/trend_analysis.png', dpi=300, bbox_inches='tight')
        plt.show()
    
    def create_interactive_dashboard(self):
        """Create interactive dashboard using Plotly"""
        # Create subplots
        fig = make_subplots(
            rows=2, cols=2,
            subplot_titles=('Monthly Revenue Trend', 'Category Performance', 
                          'Regional Distribution', 'Customer Segments'),
            specs=[[{"secondary_y": False}, {"secondary_y": False}],
                   [{"type": "pie"}, {"secondary_y": True}]]
        )
        
        # 1. Monthly Revenue Trend
        monthly_revenue = self.df.groupby([self.df['date'].dt.year, self.df['date'].dt.month])['total'].sum()
        monthly_dates = [f'{year}-{month:02d}-01' for year, month in monthly_revenue.index]
        
        fig.add_trace(
            go.Scatter(x=monthly_dates, y=monthly_revenue.values, 
                      mode='lines+markers', name='Revenue',
                      line=dict(color='blue', width=3),
                      marker=dict(size=8)),
            row=1, col=1
        )
        
        # 2. Category Performance
        category_revenue = self.df.groupby('category')['total'].sum().sort_values(ascending=False)
        
        fig.add_trace(
            go.Bar(x=category_revenue.values, y=category_revenue.index,
                   orientation='h', name='Category Revenue',
                   marker_color='lightblue'),
            row=1, col=2
        )
        
        # 3. Regional Distribution
        regional_revenue = self.df.groupby('region')['total'].sum()
        
        fig.add_trace(
            go.Pie(labels=regional_revenue.index, values=regional_revenue.values,
                   name="Regional Distribution"),
            row=2, col=1
        )
        
        # 4. Customer Segments (dual axis)
        segment_data = self.df.groupby('customer_segment').agg({
            'total': 'sum',
            'date': 'count'
        }).rename(columns={'date': 'transactions'})
        
        fig.add_trace(
            go.Bar(x=segment_data.index, y=segment_data['total'],
                   name='Revenue', marker_color='lightcoral'),
            row=2, col=2
        )
        
        fig.add_trace(
            go.Scatter(x=segment_data.index, y=segment_data['transactions'],
                      mode='lines+markers', name='Transactions',
                      line=dict(color='green', width=3),
                      marker=dict(size=10), yaxis='y2'),
            row=2, col=2, secondary_y=True
        )
        
        # Update layout
        fig.update_layout(
            title_text="Interactive Sales Dashboard",
            title_x=0.5,
            height=800,
            showlegend=True
        )
        
        # Update axes labels
        fig.update_xaxes(title_text="Month", row=1, col=1)
        fig.update_yaxes(title_text="Revenue ($)", row=1, col=1)
        
        fig.update_xaxes(title_text="Revenue ($)", row=1, col=2)
        fig.update_yaxes(title_text="Category", row=1, col=2)
        
        fig.update_xaxes(title_text="Customer Segment", row=2, col=2)
        fig.update_yaxes(title_text="Revenue ($)", row=2, col=2)
        fig.update_yaxes(title_text="Transactions", row=2, col=2, secondary_y=True)
        
        # Save and show
        fig.write_html('dashboard_plots/interactive_dashboard.html')
        fig.show()
        
        return fig
    
    def create_executive_summary(self):
        """Create executive summary visualization"""
        fig, axes = plt.subplots(2, 2, figsize=(16, 10))
        fig.suptitle('Executive Summary - Key Metrics', fontsize=18, fontweight='bold')
        
        # Calculate key metrics
        total_revenue = self.df['total'].sum()
        total_profit = self.df['profit_margin'].sum()
        avg_order_value = self.df['total'].mean()
        total_transactions = len(self.df)
        
        # 1. Key Metrics Cards (using bar chart to simulate cards)
        metrics = ['Total Revenue', 'Total Profit', 'Avg Order Value', 'Transactions']
        values = [total_revenue/1000, total_profit/1000, avg_order_value, total_transactions/1000]
        colors = ['#1f77b4', '#ff7f0e', '#2ca02c', '#d62728']
        
        bars = axes[0, 0].bar(metrics, values, color=colors)
        axes[0, 0].set_title('Key Performance Indicators', fontweight='bold')
        axes[0, 0].set_ylabel('Value (K = thousands)')
        
        # Add value labels on bars
        for bar, value, metric in zip(bars, values, metrics):
            height = bar.get_height()
            if metric == 'Avg Order Value':
                label = f'${value:.2f}'
            else:
                label = f'{value:.1f}K'
            axes[0, 0].text(bar.get_x() + bar.get_width()/2., height,
                           label, ha='center', va='bottom', fontweight='bold')
        
        # 2. Top 5 Categories
        top_categories = self.df.groupby('category')['total'].sum().sort_values(ascending=False).head(5)
        
        wedges, texts, autotexts = axes[0, 1].pie(top_categories.values, labels=top_categories.index,
                                                 autopct='%1.1f%%', startangle=90)
        axes[0, 1].set_title('Top 5 Categories by Revenue', fontweight='bold')
        
        # 3. Monthly Performance
        monthly_performance = self.df.groupby(self.df['date'].dt.month).agg({
            'total': 'sum',
            'profit_margin': 'sum'
        })
        
        months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
                 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
        
        x = np.arange(len(monthly_performance))
        width = 0.35
        
        bars1 = axes[1, 0].bar(x - width/2, monthly_performance['total']/1000, width, 
                              label='Revenue', color='skyblue')
        bars2 = axes[1, 0].bar(x + width/2, monthly_performance['profit_margin']/1000, width,
                              label='Profit', color='lightcoral')
        
        axes[1, 0].set_title('Monthly Revenue vs Profit', fontweight='bold')
        axes[1, 0].set_xlabel('Month')
        axes[1, 0].set_ylabel('Amount ($K)')
        axes[1, 0].set_xticks(x)
        axes[1, 0].set_xticklabels([months[i-1] for i in monthly_performance.index])
        axes[1, 0].legend()
        
        # 4. Growth Indicators
        # Calculate month-over-month growth
        monthly_revenue = self.df.groupby([self.df['date'].dt.year, self.df['date'].dt.month])['total'].sum()
        growth_rates = monthly_revenue.pct_change().dropna()
        
        # Create growth trend
        axes[1, 1].plot(range(len(growth_rates)), growth_rates.values * 100, 
                       marker='o', linewidth=2, markersize=6, color='green')
        axes[1, 1].axhline(y=0, color='red', linestyle='--', alpha=0.7)
        axes[1, 1].set_title('Month-over-Month Growth Rate', fontweight='bold')
        axes[1, 1].set_xlabel('Month')
        axes[1, 1].set_ylabel('Growth Rate (%)')
        axes[1, 1].grid(True, alpha=0.3)
        
        # Fill positive and negative areas
        axes[1, 1].fill_between(range(len(growth_rates)), growth_rates.values * 100, 0,
                               where=(growth_rates.values >= 0), color='green', alpha=0.3, label='Positive Growth')
        axes[1, 1].fill_between(range(len(growth_rates)), growth_rates.values * 100, 0,
                               where=(growth_rates.values < 0), color='red', alpha=0.3, label='Negative Growth')
        axes[1, 1].legend()
        
        plt.tight_layout()
        plt.savefig('dashboard_plots/executive_summary.png', dpi=300, bbox_inches='tight')
        plt.show()

# Usage example
def create_comprehensive_dashboard():
    """Create comprehensive business dashboard"""
    print("Creating comprehensive business dashboard...")
    
    # Initialize analyzer and dashboard
    from sales_analyzer import SalesAnalyzer  # Assuming previous code is in sales_analyzer.py
    analyzer = SalesAnalyzer()
    dashboard = BusinessDashboard(analyzer)
    
    print("Creating revenue dashboard...")
    dashboard.create_revenue_dashboard()
    
    print("Creating trend analysis plots...")
    dashboard.create_trend_analysis_plots()
    
    print("Creating interactive dashboard...")
    dashboard.create_interactive_dashboard()
    
    print("Creating executive summary...")
    dashboard.create_executive_summary()
    
    print("Dashboard creation completed! Check the 'dashboard_plots' folder for saved visualizations.")

if __name__ == "__main__":
    # Run dashboard creation
    create_comprehensive_dashboard()
```

This covers the core Data Analytics specialization content, focusing on practical business applications rather than academic theory. The examples show how to transform raw data into actionable business insights through comprehensive analysis and compelling visualizations.

<function_calls>
<invoke name="taskStatus">
<parameter name="status">completed