# Secure-Bank-Web-App-

## Project Overview
This project demonstrates the design of a secure AWS VPC for a bank-style web application using public and private subnets.

## Business Scenario
A bank needs a public customer-facing website, but its admin backend and database should not be directly accessible from the internet.

## Architecture
- VPC: 10.0.0.0/16
- Public Subnet: 10.0.1.0/24
- Private Subnet: 10.0.128.0/20
- Internet Gateway
- Public Route Table
- Private Route Table

## Architecture Diagram
```text
Internet
   |
Internet Gateway
   |
Public Route Table
   |
Public Subnet
   |
Private Subnet / Backend Area
