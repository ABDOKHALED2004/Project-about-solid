# Project-about-solid
from abc import ABC, abstractmethod

class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email


class EmailService:
    def send_email(self, user, message):
        print(f"Sending email to {user.email}: {message}")


class Discount(ABC):
    @abstractmethod
    def calculate(self, total):
        pass


class PercentageDiscount(Discount):
    def __init__(self, percentage):
        self.percentage = percentage

    def calculate(self, total):
        return total * (1 - self.percentage / 100)


class FixedDiscount(Discount):
    def __init__(self, amount):
        self.amount = amount

    def calculate(self, total):
        return max(0, total - self.amount)


class PaymentProcessor(ABC):
    @abstractmethod
    def process_payment(self, amount):
        pass


class CreditCardProcessor(PaymentProcessor):
    def process_payment(self, amount):
        print(f"Processing credit card payment of ${amount}")


class PayPalProcessor(PaymentProcessor):
    def process_payment(self, amount):
        print(f"Processing PayPal payment of ${amount}")


class Printable(ABC):
    @abstractmethod
    def print(self):
        pass


class Report(Printable):
    def __init__(self, content):
        self.content = content

    def print(self):
        print(f"Report Content: {self.content}")


class Order:
    def __init__(self, user, total, discount: Discount, payment_processor: PaymentProcessor):
        self.user = user
        self.total = total
        self.discount = discount
        self.payment_processor = payment_processor

    def finalize_order(self):
        discounted_total = self.discount.calculate(self.total)
        print(f"Total after discount: ${discounted_total}")
        self.payment_processor.process_payment(discounted_total)


if __name__ == "__main__":
    user = User("John Doe", "john@example.com")
    email_service = EmailService()

    discount = PercentageDiscount(10)
    payment_processor = CreditCardProcessor()

    order = Order(user, 100, discount, payment_processor)
    order.finalize_order()

    report = Report("This is a monthly report.")
    report.print()
