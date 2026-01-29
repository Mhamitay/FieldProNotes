# RiseFSM Business Guide

## Overview
RiseFSM is a Field Service Management system for oil & gas service companies.

## Core Business Entities

### Cost Items
- Master service catalog
- Defines what can be charged to customers
- Classifications: Tool, Person, Fleet, General

### Price Items  
- Customer-specific pricing
- Contract rates override base pricing
- Regional and zone-based pricing

### Jobs
- Central work orders
- Status lifecycle: Quote ? Pending ? Active ? Complete ? Invoiced
- Contains all job-related data

### Tickets
- Billable line items
- Types: Standard, Bid Items (require approval), Additional Items
- Links to equipment and personnel usage

### Load Out
- Equipment deployment tracking
- Links physical assets to billing
- Usage tracking for asset utilization

### Quotes
- Customer proposals and estimates
- Multiple versions per opportunity
- Convert to jobs when approved

## Business Workflows

### Sales Process
1. Lead Management
2. Quote Development
3. Contract Negotiation
4. Award

### Operations Process
1. Job Planning
2. Resource Allocation
3. Equipment Deployment
4. Field Operations
5. Documentation

### Financial Process
1. Time & Material Tracking
2. Billing
3. Invoice Generation
4. Payment Collection

## Key Business Rules

### Pricing Hierarchy
Cost Items ? Price Items ? Tickets
Base catalog ? Customer rates ? Actual billing

### Approval Workflows
- Standard tickets: Auto-approved
- Bid items: Require management approval
- Additional items: Emergency work tracking

### Equipment Integration
Load Out deployment automatically creates billing opportunities
Physical asset usage tracked and converted to revenue

## Industry Context

### Service Types
- Well Intervention: Remedial well work
- Wireline: Logging and precision operations  
- Completion: Well completion and fracturing

### Challenges
- Remote locations with poor connectivity
- High-value equipment ($500K - $20M per unit)
- Safety-critical operations
- Complex contracts and pricing

### Business Model
- Contract-based pricing
- Equipment utilization focus
- Expertise premium pricing
- Risk management critical
