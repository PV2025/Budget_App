# Budget_App

def create_spend_chart(categories):
    title = "Percentage spent by category\n"

    # Step 1: Get total withdrawals
    spent = []
    for cat in categories:
        total = sum(-entry['amount'] for entry in cat.ledger if entry['amount'] < 0)
        spent.append(total)

    total_spent = sum(spent)
    percentages = [int((s / total_spent) * 10) * 10 for s in spent]

    # Step 2: Build bar chart lines
    chart = ""
    for i in range(100, -1, -10):
        line = f"{i:>3}|"
        for p in percentages:
            line += " o " if p >= i else "   "
        line += " \n"  # NOTE the trailing space before newline
        chart += line

    # Step 3: Add horizontal line
    dashes = "-" * (len(categories) * 3 + 1)
    chart += f"    {dashes}\n"

    # Step 4: Category name labels vertically
    max_len = max(len(cat.name) for cat in categories)
    for i in range(max_len):
        line = "     "  # 5 spaces (4 indent + 1)
        for cat in categories:
            line += (cat.name[i] if i < len(cat.name) else " ") + "  "
        line += "\n"  # preserve trailing spaces
        chart += line

    return title + chart.rstrip('\n')  # Only remove final newline
