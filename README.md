from datetime import date

expenses = []

print("===== PERSONAL EXPENSE TRACKER =====")

while True:
    try:
        budget = float(input("Enter your monthly budget: "))
        if budget > 0:
            break
        print("Enter a positive budget.")
    except ValueError:
        print("Numbers only!")

while True:
    print("\nSelect Expense Category")
    print("1. Food")
    print("2. Travel")
    print("3. Books")
    print("4. Other")

    choice = input("Enter choice (1-4): ")

    if choice == "1":
        category = "Food"
    elif choice == "2":
        category = "Travel"
    elif choice == "3":
        category = "Books"
    elif choice == "4":
        category = "Other"
    else:
        print("Invalid choice!")
        continue

    while True:
        try:
            amount = float(input("Enter expense amount: "))
            if amount > 0:
                break
            print("Enter positive amount only.")
        except ValueError:
            print("Numbers only!")

    expense_date = str(date.today())

    expenses.append({
        "date": expense_date,
        "category": category,
        "amount": amount
    })

    with open("expenses.txt", "a") as file:
        file.write(
            expense_date + " - " + category
            + " - " + str(amount) + "\n"
        )

    choice = input("Add another expense? (yes/no): ")

    if choice.lower() != "yes":
        break

total = sum(item["amount"] for item in expenses)

print("\n===== EXPENSE REPORT =====")

for item in expenses:
    print("Date:", item["date"])
    print("Category:", item["category"])
    print("Amount:", item["amount"])
    print("--------------------")

print("Total Expenses:", total)
print("Remaining Balance:", budget - total)

print("\n===== SAVED EXPENSE HISTORY =====")

with open("expenses.txt", "r") as file:
    print(file.read())

print("\n===== CATEGORY WISE REPORT =====")

category_total = {}

for item in expenses:
    category = item["category"]
    amount = item["amount"]

    if category in category_total:
        category_total[category] += amount
    else:
        category_total[category] = amount

for category, amount in category_total.items():
    print(category, ":", amount)

print("\nTotal Number of Expenses:", len(expenses))

if total > budget:
    print("Warning: Budget Exceeded!")
elif total == budget:
    print("Your budget is fully used.")
else:
    print("You are within your budget.")

highest_expense = max(expenses, key=lambda x: x["amount"])
lowest_expense = min(expenses, key=lambda x: x["amount"])

print("\nHighest Expense:", highest_expense["category"],
      highest_expense["amount"])

print("Lowest Expense:", lowest_expense["category"],
      lowest_expense["amount"])

average_expense = total / len(expenses)

print("Average Expense:", average_expense)

print("\n===== FINAL SUMMARY =====")
print("Monthly Budget:", budget)
print("Total Expenses:", total)
print("Remaining Balance:", budget - total)
print("Number of Expenses:", len(expenses))
print("Average Expense:", average_expense)

