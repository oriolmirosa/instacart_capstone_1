# Capstone Project proposal

1. PROBLEM

Online grocery shopping poses particular challenges that are rarely encountered in other sectors:

* Customers do not have the same amount of patience when looking for items online compared to physical shopping, and therefore there is more danger of frustration and attrition
* In online stores, there are no readily available human workers to aid customers who cannot find the product they are looking for
* In physical stores, there is a structured material infrastructure to guide shoppers, who can find items they didn't know they needed just by seeing them as they walk through the aisles. In online stores, there is less exposure to other products (smaller visual field and need to scroll or navigate to a new page), and therefore the choice of what products to show customers is of critical importance to ensure a satisfactory and complete shopping experience

The key advantage of online stores is that, unlike their physical counterparts, the structure of the shopping experience can be customized to the needs and preferences of different customers. The goal of this project is to identify between 5 and 10 types of online Instacart shoppers and to predict the likelihood that a new customer will belong to each of these groups.

2. CLIENT

Who is your client and why do they care about this problem? In other words, what will your client DO or DECIDE based on your analysis that they wouldn’t have otherwise?

The client for this project is Instcart, and in particular key decisionmakers within the firm who participate in determining the user experience in the online store. Thanks to the findings of this project, these decisionmakers will be able to identify who their customers are early in the shopping process and thus build a customized online shopping experience that shows them relevant products and is adapted to their expectations and needs. This will contribute to improving customer experience and maximizing revenue.

3. DATA

The data used for this project is Instacart's public grocery shopping dataset, which can be found on the company's website:

https://www.instacart.com/datasets/grocery-shopping-2017

The dataset contains data on 3.4 million orders from 206,000 users involving 50,000 products.

4. APPROACH TO SOLVING THE PROBLEM

I will solve this problem in two steps. First, I will identify groups of consumers according to their ordering patterns. This is a clustering problem. Using the information of the orders of each customer, I will employ machine learning algorithms to identify sets of consumers with distinctive ordering patterns.

Second, I will predict the likelihood that a given customer belongs to one of the consumption pattern groups. Since the goal is to be able to identify as quickly as possible the group to which the consumer belongs, I will predict group membership with as little information as possible. This means focusing on the first few orders for each customer, and even the first few products that they add to the shopping basket. I will undertake this task with classification machine learning algorithms that will use early shopping process information to predict the group to which the customer belongs.

5. DELIVERABLES

The final product of this project will be presented in the form of a GitHub repository that will include:

* All the code used to perform the analysis
* A short memo discussing the findings
* A slide deck presenting the main conclusions of the analysis as well as the main visualizations

