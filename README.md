# Uber Supply-Demand Gap Analysis

## Table of Contents

* [Project Overview](#project-overview)
* [Data Cleaning and Preparation](#data-cleaning-and-preparation)
* [Excel Dashboard Summary](#excel-dashboard-summary)
* [SQL Insights](#sql-insights)
* [Python EDA Insights](#python-eda-insights)
* [Data Sources](#data-sources)
* [Recommendations](#recommendations)

## Project Overview

This project investigates the Uber supply-demand gap using the data. Through a
combination of Excel dashboards, SQL queries, and Python-based EDA, we identify periods
of high demand and frequent trip failures. Our goal is to uncover patterns that explain user
dissatisfaction, driver availability, and canceled rides, and provide actionable insights.

## Data Cleaning and Preparation

The dataset was cleaned using Excel and Python Pandas.

Key steps:
- Converted inconsistent timestamp formats to datetime.
- Extracted date, hour, and weekday from request timestamps.
- Classified requests into time slots: Early Morning, Morning, Afternoon, Evening, Night.
- Calculated Trip Duration (only for completed rides).
- Created Status Category column to label trips as Success or Failure.
- Handled missing values in Drop timestamp by interpreting them as failed rides.

## Excel Dashboard Summary

### Chart 1: Status by Time Slot

This chart compares the count of each ride status across time slots. It highlights that most
trip failures (No Cars Available and Cancelled) occur in the Early Morning and Night. This
insight informs when driver supply needs to be increased.

![status vs time slot](status-vs-time-slot.png)
