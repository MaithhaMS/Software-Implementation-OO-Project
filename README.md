# Software-Implementation-OO-Project
# Artwork Management
class Artwork:
    def __init__(self, title, artist, creationDate, historicalSignificance, location):
        self.title = title
        self.artist = artist
        self.creationDate = creationDate
        self.historicalSignificance = historicalSignificance
        self.location = location
  
    def display_info(self):
        print(f"Title: {self.title}")
        print(f"Artist: {self.artist}")
        print(f"Date of Creation: {self.creationDate}")
        print(f"Historical Significance: {self.historicalSignificance}")
        print(f"Location: {self.location}")

class Exhibition(Artwork):
    def __init__(self, title, artist, creationDate, historicalSignificance, location, duration):
        super().__init__(title, artist, creationDate, historicalSignificance, location)
        self.duration = duration

    def display_info(self):
        super().display_info()
        print(f"Duration: {self.duration}")

class ArtworkCatalog:
    def __init__(self):
        self.artworks = []
        self.artifacts = []
        self.exhibitions = []

    def add_artwork(self, artwork):
       self.artworks.append(artwork)

    def add_artifact(self, artifact):
        self.artifacts.append(artifact)

    def add_exhibition(self, exhibition):
        self.exhibitions.append(exhibition)

    def display_catalog(self):
        print("Artworks:")
        for artwork in self.artworks:
            artwork.display_info()
        print("\nArtifacts:")
        for artifact in self.artifacts:
            artifact.display_info()
        print("\nExhibitions:")
        for exhibition in self.exhibitions:
            exhibition.display_info()


# Visitor Management
class Visitor:
    def __init__(self, visitorID, name, age, nationality, isTeacher=False, isStudent=False):
        self.visitorID = visitorID
        self.name = name
        self.age = age
        self.nationality = nationality
        self.isTeacher = isTeacher
        self.isStudent = isStudent

    def display_visitor_info(self):
        print(f"Visitor ID: {self.visitorID}")
        print(f"Name: {self.name}")
        print(f"Age: {self.age}")
        print(f"Nationality: {self.nationality}")
        print(f"Is Teacher: {'Yes' if self.isTeacher else 'No'}")
        print(f"Is Student: {'Yes' if self.isStudent else 'No'}")

class VisitorManagementSystem:
    def __init__(self):
        self.visitors = []
        self.tickets = []

    def add_visitor(self, visitor):
        self.visitors.append(visitor)

    def purchase_ticket(self, ticket):
        self.tickets.append(ticket)

    def display_all_tickets(self):
         for ticket in self.tickets:
            ticket.display_ticket_info()


# Ticketing and Pricing
class priceStrategy:
    standardPrice = 63
    VAT_Rate = 0.05

    def calculate_ticket_price(visitor, isGroup=False, specialEventPrice=None):
        if visitor.age <= 18 or visitor.age >= 60 or visitor.isTeacher or visitor.isStudent:
            return 0 #Free price 
        price = priceStrategy.standardPrice if specialEventPrice is None else specialEventPrice
        if isGroup:
            price *= 0.5
        final_price = price * (1 + priceStrategy.VAT_Rate)
        return final_price

class Ticket:
    def __init__(self, ticketID, visitor, eventType, date, location, isGroup=False, specialEventPrice=None):
        self.ticketID = ticketID
        self.visitor = visitor
        self.eventType = eventType
        self.date = date
        self.location = location
        self.isGroup = isGroup
        self.specialEventPrice = specialEventPrice
        self.price = priceStrategy.calculate_ticket_price(visitor, isGroup, specialEventPrice)

    def display_ticket_info(self):
        print(f"Ticket ID: {self.ticketID}")
        print(f"Visitor: {self.visitor.name}")
        print(f"Event Type: {self.eventType}")
        print(f"Date: {self.date}")
        print(f"Location: {self.location}")
        print(f"Price: {self.price} AED")

class ExhibitionTicket(Ticket):
    def __init__(self, ticketID, visitor, eventType, date, location, exhibitionName, isGroup=False, specialEventPrice=None):
        super().__init__(ticketID, visitor, eventType, date, location, isGroup, specialEventPrice)
        self.exhibitionName = exhibitionName 

class TourTicket(Ticket):
    def __init__(self, ticketID, visitor, eventType, date, location, tourDate, groupSize, guideName, isGroup=False, specialEventPrice=None):
        super().__init__(ticketID, visitor, eventType, date, location, isGroup, specialEventPrice)
        self.tourDate = tourDate  
        self.groupSize = groupSize 
        self.guideName = guideName  

class EventTicket(Ticket):
    def __init__(self, ticketID, visitor, eventType, date, location, eventName, eventDuration, isGroup=False, specialEventPrice=None):
        super().__init__(ticketID, visitor, eventType, date, location, isGroup, specialEventPrice)
        self.eventName = eventName  
        self.eventDuration = eventDuration  


# Test cases
def test_artwork_catalog():
    catalog = ArtworkCatalog()
    catalog.add_artwork(Artwork("Mona Lisa", "Leonardo da Vinci", "1503", "Renaissance masterpiece", "Permanent Gallery"))
    catalog.add_artifact(Artwork("Dhow in the Moonlight", "Abdul Qader Al Rais", "1989", "Captures traditional Emirati sailing vessels", "UAE Cultural Exhibit"))
    catalog.add_exhibition(Exhibition("Impressionists on Display", "Various Artists", "2024", "Showcases the Impressionist movement", "Exhibition Hall A", "April - September 2024"))
    catalog.display_catalog()


def test_visitor_management_system():
    vms = VisitorManagementSystem()
    visitor = Visitor("T123", "Maitha Mayed", 19, "Emirate", False, True)
    vms.add_visitor(visitor)
    exhibition_ticket = Ticket("T111", visitor, "Exhibition", "2024-04-18", "Permanent Gallery")
    vms.purchase_ticket(exhibition_ticket)
    vms.display_all_tickets()

def test_pricing_strategy():
    visitor_adult = Visitor("TX12", "Abdulla", 35, "Saudi Arabia", False, False)
    visitor_senior = Visitor("TM13", "Salama", 65, "Qatar", False, False)
    visitor_teacher = Visitor("TFF2", "Mohd", 42, "India", True, False)
    ticket_adult = Ticket("T222", visitor_adult, "Exhibition", "2024-04-11", "Temporary Gallery")
    ticket_senior = Ticket("T333", visitor_senior, "Exhibition", "2024-04-11", "Temporary Gallery")
    ticket_teacher = Ticket("T444", visitor_teacher, "Exhibition", "2024-04-11", "Temporary Gallery", True)
    ticket_adult.display_ticket_info()
    ticket_senior.display_ticket_info()
    ticket_teacher.display_ticket_info()

# Running Test Functions
if __name__ == "__main__":
    test_artwork_catalog()
    test_visitor_management_system()
    test_pricing_strategy()
