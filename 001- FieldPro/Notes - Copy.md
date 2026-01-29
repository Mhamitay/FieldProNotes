```mermaid
flowchart LR
    %% Phases
    setup["Setup Phase\n- Define Cost Items\n- Create Price Books"]
    sales["Sales Phase\n- Build Quotes\n- Customer approval"]
    execution["Execution Phase\n- Deploy Load Out\n- Create/approve Tickets"]
    billing["Billing Phase\n- Tickets → Invoice\n- Taxes, GL integration"]

    %% Sub-components
    cost_items["Cost Items\n- Tools\n- People\n- Fleet\n- Other"]
    price_books["Price Books\n- Master\n- Child\n- Multi-currency"]
    quotes["Quotes\n- Multiple versions\n- Approved → Tickets"]
    loadout["Load Out\n- Equipment\n- Rentals\n- Consumables"]
    tickets["Tickets\n- Regular, Bid, Additional"]
    jobs["Jobs\n- Links quotes, tickets, loadout\n- Tracks workflow stages"]
    intelligence["System Intelligence\n- Auto-pricing, Approvals\n- Audit trails"]

    %% Connections
    setup --> sales
    sales --> execution
    execution --> billing

    setup --> cost_items
    setup --> price_books
    sales --> quotes
    execution --> loadout
    execution --> tickets

    jobs --> quotes
    jobs --> tickets
    jobs --> loadout

    tickets --> intelligence
