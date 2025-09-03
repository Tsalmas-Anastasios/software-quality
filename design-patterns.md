# Factory Pattern

**Παράδειγμα:** Θεωρούμε ένα σενάριο όπου θέλουμε να δημιουργήσουμε αντικείμενα διαφορετικού τύπου οχημάτων, όπως αυτοκίνητα, ποδήλατα και φορτηγά. Για να το υλοποιήσουμε αυτό, μπορούμε να χρησιμοποιήσουμε το πρότυπο Factory.

## Υλοποίηση

Ορίζουμε ένα interface `Vehicle`:

```java
public interface Vehicle {
    void drive();
}
```

Δημιουργούμε συγκεκριμένες υλοποιήσεις για κάθε τύπο οχήματος:

```java
public class Car implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Driving a car");
    }
}

public class Bike implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Driving a bike");
    }
}

public class Truck implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Driving a truck");
    }
}
```

Δημιουργούμε μια κλάση `VehicleFactory` για τη δημιουργία αντικειμένων:

```java
public class VehicleFactory {
    public static Vehicle getVehicle(String type) {
        switch (type) {
            case "car":
                return new Car();
            case "bike":
                return new Bike();
            case "truck":
                return new Truck();
            default:
                return null;
        }
    }
}
```

### Πλεονεκτήματα
- Ενθυλακώνει τη λογική δημιουργίας αντικειμένων, καθιστώντας τον κώδικα πιο συντηρήσιμο και ευέλικτο.
- Μειώνει τη σύζευξη μεταξύ του κώδικα και των αντικειμένων που δημιουργούνται.
- Αποφεύγει τη στενή σύζευξη μεταξύ συγκεκριμένων κλάσεων.
- Βοηθά στη μείωση της επανάληψης του κώδικα.

### Μειονεκτήματα
- Προσθέτει πολυπλοκότητα στον κώδικα, καθώς υπάρχει ένα επιπλέον επίπεδο αφαίρεσης.
- Μπορεί να οδηγήσει σε μείωση της απόδοσης, καθώς η μέθοδος factory προσθέτει μια επιπλέον κλήση μεθόδου.

---

# Composite Pattern

**Παράδειγμα:** Θεωρούμε ότι φτιάχνουμε έναν File Explorer. Θέλουμε να αναπαραστήσουμε αρχεία και καταλόγους στο πρόγραμμά μας. Τα αρχεία και οι κατάλογοι μπορούν να περιέχουν άλλα αρχεία και καταλόγους.

## Υλοποίηση

Ορίζουμε μια αφηρημένη κλάση `Component`:

```java
public abstract class Component {
    protected String name;
    
    public Component(String name) {
        this.name = name;
    }

    public abstract void add(Component component);
    public abstract void remove(Component component);
    public abstract void display();
}
```

Δημιουργούμε τις κλάσεις `Directory` και `File`:

```java
// ----------------- DIRECTORY CLASS -----------------
public class Directory extends Component {
    private List<Component> components = new ArrayList<>();

    public Directory(String name) {
        super(name);
    }

    @Override
    public void add(Component component) {
        components.add(component);
    }

    @Override
    public void remove(Component component) {
        components.remove(component);
    }

    @Override
    public void display() {
        System.out.println("Directory: " + name);
        
        for (Component component : components) {
            component.display();
        }
    }
}

// ----------------- FILE CLASS -----------------
public class File extends Component {
    public File(String name) {
        super(name);
    }

    @Override
    public void add(Component component) {
        throw new UnsupportedOperationException();
    }

    @Override
    public void remove(Component component) {
        throw new UnsupportedOperationException();
    }

    @Override
    public void display() {
        System.out.println("File: " + name);
    }
}
```

Παράδειγμα χρήσης:

```java
public static void main(String[] args) {
    Component root = new Directory("root");
    Component dir1 = new Directory("dir1");
    Component file1 = new File("file1");
    root.add(dir1);
    dir1.add(file1);
    root.display();
}
```

### Πλεονεκτήματα
- Μας επιτρέπει να αντιμετωπίζουμε μεμονωμένα αντικείμενα και συνθέσεις αντικειμένων με ομοιόμορφο τρόπο.
- Διευκολύνει την προσθήκη νέων τύπων συστατικών στο σύστημα.

### Μειονεκτήματα
- Αυξάνει την πολυπλοκότητα, καθώς εισάγει μια εντελώς νέα ιεραρχία.
- Μπορεί να κάνει πιο δύσκολη την κατανόηση της δομής του συστήματος.
- Μπορεί να οδηγήσει σε υπερβολικά πολύπλοκες συνθέσεις.

---

# Command Pattern

**Παράδειγμα:** Θέλουμε να δημιουργήσουμε ένα σύστημα όπου θα μπορούμε να εκτελούμε λειτουργίες όπως "Άνοιγμα", "Κλείσιμο" και "Αποθήκευση" σε πολλά αντικείμενα.

## Υλοποίηση

Δημιουργούμε ένα interface `Command`:

```java
interface Command {
    void execute(Object target);
}
```

Κλάσεις εντολών:

```java
// ----------------- OPEN CLASS -----------------
class OpenCommand implements Command {
    @Override
    public void execute(Object target) {
        System.out.println("Open Command executed on " + target);
    }
}

// ----------------- CLOSE CLASS -----------------
class CloseCommand implements Command {
    @Override
    public void execute(Object target) {
        System.out.println("Close Command executed on " + target);
    }
}

// ----------------- SAVE CLASS -----------------
class SaveCommand implements Command {
    @Override
    public void execute(Object target) {
        System.out.println("Save Command executed on " + target);
    }
}
```

Invoker:

```java
class Invoker {
    private Command command;

    public Invoker(Command command) {
        this.command = command;
    }

    public void executeCommand(Object target) {
        command.execute(target);
    }
}
```

Client:

```java
class Client {
    public static void main(String[] args) {
        Object file = new Object();
        Command openCommand = new OpenCommand();
        Command closeCommand = new CloseCommand();
        Command saveCommand = new SaveCommand();
        
        Invoker openInvoker = new Invoker(openCommand);
        Invoker closeInvoker = new Invoker(closeCommand);
        Invoker saveInvoker = new Invoker(saveCommand);
        
        openInvoker.executeCommand(file);
        closeInvoker.executeCommand(file);
        saveInvoker.executeCommand(file);
    }
}
```

### Πλεονεκτήματα
- Παρέχει έναν ευέλικτο τρόπο για την προσθήκη νέας συμπεριφοράς χωρίς αλλαγή στον υπάρχοντα κώδικα.
- Διαχωρίζει το UI από τη λογική του συστήματος.
- Μειώνει τις εξαρτήσεις μεταξύ αντικειμένων.

---

# Observer Pattern

Το Observer Pattern είναι ένα πρότυπο όπου ένα αντικείμενο (Subject) διατηρεί έναν κατάλογο των Observers του και τους ειδοποιεί για αλλαγές στην κατάστασή του.

**Παράδειγμα:** Ένας μετεωρολογικός σταθμός μεταδίδει πληροφορίες καιρού σε συσκευές (smartphone, tablet, web app).

## Υλοποίηση

Ορίζουμε το interface παρατηρητή:

```java
public interface WeatherObserver {
    void update(float temperature, float humidity, float pressure);
}
```

Κλάση Subject:

```java
public class WeatherStation {
    private List<WeatherObserver> observers = new ArrayList<>();
    private float temperature;
    private float humidity;
    private float pressure;

    public void addObserver(WeatherObserver observer) {
        observers.add(observer);
    }

    public void removeObserver(WeatherObserver observer) {
        observers.remove(observer);
    }

    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        notifyObservers();
    }

    private void notifyObservers() {
        for (WeatherObserver observer : observers) {
            observer.update(temperature, humidity, pressure);
        }
    }
}
```

Observers:

```java
public class SmartphoneDisplay implements WeatherObserver {
    @Override
    public void update(float temperature, float humidity, float pressure) {
        System.out.println("Smartphone: " + temperature + "°C, " + humidity + "%, " + pressure + "hPa");
    }
}

public class TabletDisplay implements WeatherObserver {
    @Override
    public void update(float temperature, float humidity, float pressure) {
        System.out.println("Tablet: " + temperature + "°C, " + humidity + "%, " + pressure + "hPa");
    }
}

public class WebAppDisplay implements WeatherObserver {
    @Override
    public void update(float temperature, float humidity, float pressure) {
        System.out.println("WebApp: " + temperature + "°C, " + humidity + "% humidity, " + pressure + "hPa");
    }
}
```

Παράδειγμα χρήσης:

```java
WeatherStation weatherStation = new WeatherStation();
SmartphoneDisplay smartphoneDisplayObserver = new SmartphoneDisplay();

weatherStation.addObserver(smartphoneDisplayObserver);

TabletDisplay tabletDisplayObserver = new TabletDisplay();
weatherStation.addObserver(tabletDisplayObserver);
