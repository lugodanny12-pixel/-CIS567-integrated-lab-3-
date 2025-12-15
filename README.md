# -CIS567-integrated-lab-3-
Week 9 Assignment
def read_customer_data(filename):
    """Read and return data from filename as a list of lists (name, state, debt)"""
    names = []
    states = []
    debts = []

    with open(filename) as f:
        rows = f.readlines()
    for row in rows:
        row = row.split(',')
        names.append(row[0])
        states.append(row[1])
        debts.append(float(row[2].strip()))
    return names, states, debts


def run_report():
    # number of rows to consider
    num_customers = int(input())

    names, states, debts = read_customer_data("CustomerData.csv")

    # Step 1: read debt limit, search phrase, and state abbreviation
    debt_limit = int(input())
    search_phrase = input().strip()
    state_abbrev = input().strip()

    # ---------- U.S. Report ----------
    print("U.S. Report")
    print(f"Customers: {num_customers}")

    # Step 2: highest debt (overall)
    max_debt = -1.0
    max_name = ""
    for i in range(num_customers):
        if debts[i] > max_debt:
            max_debt = debts[i]
            max_name = names[i]
    print(f"Highest debt: {max_name}")

    # Step 3: count names starting with search_phrase (overall)
    count_prefix_us = 0
    for i in range(num_customers):
        if names[i].startswith(search_phrase):
            count_prefix_us += 1
    print(f'Customer names that start with "{search_phrase}": {count_prefix_us}')

    # Step 4: count over debt_limit and debt-free (overall)
    count_over_limit_us = 0
    count_debt_free_us = 0
    for i in range(num_customers):
        if debts[i] > debt_limit:
            count_over_limit_us += 1
        if debts[i] == 0:
            count_debt_free_us += 1

    print(f"Customers with debt over ${debt_limit}: {count_over_limit_us}")
    print(f"Customers debt free: {count_debt_free_us}")

    # ---------- State Report ----------
    print()
    print(f"{state_abbrev} Report")

    state_count = 0
    state_max_debt = -1.0
    state_max_name = ""

    state_prefix_count = 0
    state_over_limit = 0
    state_debt_free = 0

    for i in range(num_customers):
        if states[i] == state_abbrev:
            state_count += 1

            if debts[i] > state_max_debt:
                state_max_debt = debts[i]
                state_max_name = names[i]

            if names[i].startswith(search_phrase):
                state_prefix_count += 1

            if debts[i] > debt_limit:
                state_over_limit += 1

            if debts[i] == 0:
                state_debt_free += 1

    print(f"Customers: {state_count}")
    print(f"Highest debt: {state_max_name}")
    print(f'Customer names that start with "{search_phrase}": {state_prefix_count}')
    print(f"Customers with debt over ${debt_limit}: {state_over_limit}")
    print(f"Customers debt free: {state_debt_free}")
if __name__ == '__main__':
    run_report()
