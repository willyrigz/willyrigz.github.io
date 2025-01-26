# Transforming Emergency Services with Data Insights: A Study on Fire and Ambulance Operations in Dublin

## Background and Overview
Dublin City Council has published a comprehensive dataset capturing fire and ambulance operations over a decade (2012–2023). These emergency services are critical to public safety, yet their efficiency depends heavily on data-informed decision-making. However, fragmented records, missing values, and inconsistent naming conventions have limited the utility of this data.

This project aims to clean, consolidate, and model this historical data to uncover actionable insights. By enabling operational leaders to identify trends, bottlenecks, and performance metrics, we support smarter resource allocation, better planning, and faster response times for emergencies.

For technical details on data cleaning and preparation, see the accompanying [Python notebook](https://github.com/willyrigz/DublinEmergencyServicesAnalysis/blob/main/DataCleaning_and_Preparation.ipynb).

---

## Data Structure Overview
The dataset consists of incident records for fire and ambulance services in Dublin, covering over 10 years. Each record includes:

- **Incident Details**: Time of call (TOC), arrival time (IA), resolution time (CD), and descriptions.
- **Station Information**: Station names and locations.
- **Incident Classifications**:
  - **Ambulance**: Criticality codes (e.g., Alpha, Bravo, Delta).
  - **Fire**: Incident descriptions (e.g., alarms, structural fires).

The data was structured into:
- **Fact Tables**:
  - `Fact_Fire`: Fire incident response and resolution times.
  - `Fact_Ambulance`: Ambulance callout response metrics.
- **Dimension Tables**:
  - `Dim_Date`: Dates for time-based analysis.
  - `Dim_Station`: Station locations and identifiers.
  - `Dim_Criticality` and `Dim_Description`: Classifications for ambulance and fire incidents, respectively.

### Entity-Relationship Diagram
Below is the Entity-Relationship Diagram (ERD) showcasing the data model:

![Entity-Relationship Diagram](/DublinEmergencyServicesAnalysis/ERD.png)

---

## Analysis & [Interactive Dashboard Design](https://public.tableau.com/views/DublinEmergencyServices/DublinFireBrigadeCallOutsDashboard)
After cleaning and preparing the data, we embarked on a journey to uncover the stories hidden within Dublin’s fire and ambulance operations. The insights we found provide a clearer picture of the challenges, successes, and opportunities for emergency services. Here’s what the data revealed:

### The Busiest Fire Station
Imagine the demands faced by **Tallaght Fire Station**, which handled an incredible **23,396 callouts** over the analysed period, making it the busiest station in Dublin. This sheer volume highlights Tallaght’s critical role in protecting its community and underscores the importance of optimising resources for high-demand stations like this.

### The Longest Response Time
The longest response time tells a story of operational challenges. On **June 19, 2015**, **Kilbarrack Fire Station** responded to a **Fire GORSE** incident with a staggering response time of **9 hours, 24 minutes, and 43 seconds**. The details paint the picture:
- **Time of Call (TOC):** 00:31:53
- **Time in Attendance (IA):** 09:56:36
- **Date:** June 19, 2015

This might raise eyebrows, but it invites further investigation. Was this an exceptionally remote area? Did resource availability or operational delays play a role? Stakeholders can explore these questions to uncover actionable improvements.

### Trends and Peaks
The weekly activity trends for 2023 revealed several spikes, particularly during **Weeks 34, 41, 42, and 45**. What’s fascinating is how these align with key events in Dublin:
- **Week 34:** The **Dalkey Lobster Festival** and **Smithfield Fleadh** likely increased crowd densities and associated risks.
- **Week 41:** The **Dublin Theatre Festival** brought large audiences across the city.

These peaks tell us where to focus resources during high-demand periods.

### Incident Descriptions: Shifts Over Time
Some incident types saw dramatic shifts. For instance:
- **Fire Alarms** rose by **4.9% year-over-year**, reflecting heightened detection and reporting.
- Meanwhile, **Small Fires** declined by **21%**, a sign that prevention campaigns might be working.

This dual trend suggests areas for continued investment in maintaining robust alarm systems while addressing emerging fire risks.

### Why It Matters
This isn’t just about numbers; it’s about understanding what they mean for the people behind the callouts. When Tallaght responds to over 23,000 incidents, it represents thousands of lives potentially saved. When Kilbarrack takes nine hours to attend an incident, it’s a reminder of the logistical challenges emergency services face daily. Peaks during festivals and declines in small fires tell us how our community behaves and where proactive measures can make the biggest impact.

The [Tableau dashboard](https://public.tableau.com/views/DublinEmergencyServices/DublinFireBrigadeCallOutsDashboard)
 amplifies this understanding, making it accessible and actionable for decision-makers. By diving into the interactive visualisations, users can explore:
- **Yearly trends** to identify where callouts are increasing or decreasing.
- **Station workloads** to ensure equitable resource distribution.
- **Response times** to pinpoint delays and optimise performance.

With this data, Dublin Fire Brigade and Ambulance Services can plan smarter, act faster, and serve better.

---

## Conclusion
This project reminds us that behind every data point is a story of a call for help, a challenge faced, or a life saved. By understanding these stories, we equip ourselves to create a safer, more resilient Dublin. The next time Tallaght answers a call, or Kilbarrack faces a long journey to assist, we’ll know that data is driving decisions to improve outcomes for all.

Let’s keep transforming numbers into action, insights into progress, and challenges into opportunities.

---

## Links
- **Original Data Source**: [Fire and Ambulance Dataset](https://data.gov.ie/dataset/fire-brigade-and-ambulance)
- **Data Preparation**: [Python Notebook](https://github.com/willyrigz/DublinEmergencyServicesAnalysis/blob/main/DataCleaning_and_Preparation.ipynb)
- **Interactive Dashboard**: [Tableau Public Dashboard](https://public.tableau.com/views/DublinEmergencyServices/DublinFireBrigadeCallOutsDashboard)
