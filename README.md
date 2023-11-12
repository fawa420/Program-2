TOTAL_COACHES = 6
SEATS_PER_COACH = 80
EXTRA_COACHES_LAST_TRAIN = 2
RETURN_TICKET_COST = 25

class TrainJourney:
    def __init__(self):
        self.passengers_booked = 0
        self.money_collected = 0

journeys_up = [TrainJourney() for _ in range(4)]
journeys_down = [TrainJourney() for _ in range(4)]
available_seats = [TOTAL_COACHES * SEATS_PER_COACH] * 4  # Changed to 4

total_passengers = 0
total_money = 0
max_passengers = 0
max_passengers_index = -1
total_passengers_free = 0

while True:
    passengers = int(input("Enter the number of passengers (1-480) or '0' to exit: "))
    if passengers == 0:
        break
    if not 1 <= passengers <= 480:
        print("Invalid number of passengers. Please enter a value between 1 and 480.")
        continue
    train_time = int(input("Enter the train time (9, 11, 13, 15): "))
    if train_time not in [9, 11, 13, 15]:
        print("Invalid train time. Please enter a valid time.")
        continue
    return_train_time = (train_time + 1) % 24
    train_index = (train_time - 9) // 2
    if train_time == 11:
        train_index = 1

    ticket_choice = int(input("Do you want tickets for the journey up (1), down (2), or both (3)?: "))
    ticket_cost = 0
    free_tickets = 0

    if ticket_choice == 1 or ticket_choice == 3:
        if available_seats[train_index] >= passengers:
            available_seats[train_index] -= passengers
            remaining_passengers = passengers
            free_tickets = remaining_passengers // 10
            remaining_passengers -= free_tickets
            single_journey_cost = remaining_passengers * RETURN_TICKET_COST
            journeys_up[train_index].passengers_booked += passengers
            journeys_up[train_index].money_collected += single_journey_cost
            total_passengers += passengers
            total_money += single_journey_cost
            total_passengers_free += free_tickets
            if passengers > max_passengers:
                max_passengers = passengers
                max_passengers_index = train_index
            ticket_cost += single_journey_cost
            print(f"Tickets booked for {passengers} passengers for the journey up.")
            print(f"{free_tickets} passengers travel for free on the journey up.")
        else:
            print("Journey up is full! Tickets cannot be booked for this journey.")

    if ticket_choice == 2 or ticket_choice == 3:
        if return_train_time == 11:
            train_index = 1
        if available_seats[train_index] >= passengers:
            available_seats[train_index] -= passengers
            remaining_passengers = passengers
            free_tickets = remaining_passengers // 10
            remaining_passengers -= free_tickets
            single_journey_cost = remaining_passengers * RETURN_TICKET_COST
            journeys_down[train_index].passengers_booked += passengers
            journeys_down[train_index].money_collected += single_journey_cost
            total_passengers += passengers
            total_money += single_journey_cost
            total_passengers_free += free_tickets
            if passengers > max_passengers:
                max_passengers = passengers
                max_passengers_index = train_index
            ticket_cost += single_journey_cost
            print(f"Tickets booked for {passengers} passengers for the journey down.")
            print(f"{free_tickets} passengers travel for free on the journey down.")
        else:
            print("Journey down is full! Tickets cannot be booked for this journey.")

    print(f"Total cost for the selected journey(s): ${ticket_cost}")

print("\nTotals for Journeys Up (Passengers / Money Collected):")
for i, journey in enumerate(journeys_up):
    print(f"Journey {i + 1}: {journey.passengers_booked} / ${journey.money_collected}")

print("\nTotals for Journeys Down (Passengers / Money Collected):")
for i, journey in enumerate(journeys_down):
    print(f"Journey {i + 1}: {journey.passengers_booked} / ${journey.money_collected}")

print(f"\nTotal Passengers for the day: {total_passengers}")
print(f"Total Money Collected for the day: ${total_money}")

if max_passengers_index != -1:
    print(f"The most popular journey of the day is Train {max_passengers_index + 1} with {max_passengers} passengers.")
else:
    print("No journeys were made today.")

print(f"Total passengers who travel for free: {total_passengers_free}")
