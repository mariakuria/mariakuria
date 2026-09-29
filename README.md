import tkinter as tk


class TransactionHistory:
    def __init__(self, root):
        self.root = root

        self.transactions = [
            "Amazon - $45.99",
            "Walmart - $72.30",
            "Netflix - $15.99",
            "Starbucks - $6.50",
            "Target - $34.25",
            "Uber - $18.75",
            "Apple - $129.99",
            "Chipotle - $14.50",
            "Amazon - $22.10",
            "Walmart - $31.45"
        ]

        self.search_text = tk.StringVar()

        self.create_search_bar()
        self.create_transaction_list()

    # Creates the search bar
    def create_search_bar(self):
        search_frame = tk.Frame(self.root)
        search_frame.pack(pady=10)

        self.search_entry = tk.Entry(
            search_frame,
            textvariable=self.search_text,
            width=40
        )
        self.search_entry.pack(side=tk.LEFT)

        search_button = tk.Button(
            search_frame,
            text="🔍",
            command=self.search_transactions
        )
        search_button.pack(side=tk.LEFT)

        # Update suggestions whenever the user types
        self.search_text.trace_add(
            "write",
            self.update_suggestions
        )

    # Creates the transaction history list
    def create_transaction_list(self):
        self.transaction_list = tk.Listbox(
            self.root,
            width=50,
            height=10
        )
        self.transaction_list.pack(pady=10)

        self.display_transactions(self.transactions)

    # Filters transactions based on the search text
    def filter_transactions(self):
        query = self.search_text.get().lower()

        return [
            transaction
            for transaction in self.transactions
            if query in transaction.lower()
        ]

    # Shows the top 5 matching transactions
    def update_suggestions(self, *args):
        query = self.search_text.get()

        if query == "":
            self.hide_suggestions()
            return

        filtered_transactions = self.filter_transactions()
        suggestions = filtered_transactions[:5]

        self.show_suggestions(suggestions)

    # Displays the suggestions underneath the search bar
    def show_suggestions(self, suggestions):
        self.hide_suggestions()

        if not suggestions:
            return

        self.suggestion_list = tk.Listbox(
            self.root,
            width=50,
            height=len(suggestions)
        )
        self.suggestion_list.pack()

        for transaction in suggestions:
            self.suggestion_list.insert(
                tk.END,
                transaction
            )

    # Removes the suggestion list
    def hide_suggestions(self):
        if hasattr(self, "suggestion_list"):
            self.suggestion_list.destroy()
            del self.suggestion_list

    # Displays all transactions matching the search
    def search_transactions(self):
        filtered_transactions = self.filter_transactions()

        self.hide_suggestions()
        self.display_transactions(filtered_transactions)

    # Updates the transaction history displayed on screen
    def display_transactions(self, transactions):
        self.transaction_list.delete(0, tk.END)

        for transaction in transactions:
            self.transaction_list.insert(
                tk.END,
                transaction
            )


root = tk.Tk()
root.title("Transaction History")

app = TransactionHistory(root)

root.mainloop()
