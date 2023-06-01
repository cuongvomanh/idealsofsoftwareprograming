You are a software develop assistant using programing language Java. Requirement: Calculate fee for payment, fee type is field of payment. There is two fee type. Fee type fixed: fee amount is fixed 2 dolar. Fee type percentage: fee amount is 50 percent of payment amount. Using replace conditional with polymorphism. Using delegate for fee calculator resolver. Using FeeRecord contains payment and fee type. Using Fee calculator resolver from config in database via Repository interface

## ChatGPT
Here's an example implementation:

First, we'll define an interface for our fee calculator:

```
public interface FeeCalculator {
    double calculateFee(double paymentAmount);
}
```

This interface has a single method, `calculateFee()` which takes in a payment amount and returns the calculated fee.

Next, we'll create two implementations of this interface - one for fixed fees and one for percentage fees:

```
public class FixedFeeCalculator implements FeeCalculator {
    private static final double FIXED_FEE_AMOUNT = 2.0;
    
    @Override
    public double calculateFee(double paymentAmount) {
        return FIXED_FEE_AMOUNT;
    }
}

public class PercentageFeeCalculator implements FeeCalculator {
    private static final double PERCENTAGE_FEE_AMOUNT = 0.5;
    
    @Override
    public double calculateFee(double paymentAmount) {
        return paymentAmount * PERCENTAGE_FEE_AMOUNT;
    }
}
```

The `FixedFeeCalculator` simply returns a fixed fee amount of $2. The `PercentageFeeCalculator` calculates the fee as a percentage of the payment amount.

We'll also create a `FeeRecord` class to hold information about the payment and fee type:

```
public class FeeRecord {
    private double paymentAmount;
    private FeeType feeType;

    // constructor, getters and setters
}
```

`FeeType` is an enum representing the two types of fees:

```
public enum FeeType {
    FIXED,
    PERCENTAGE
}
```

Now, we'll create a `FeeCalculatorResolver` class to delegate to the appropriate fee calculator based on the fee type:

```
public class FeeCalculatorResolver {
    private final Map<FeeType, FeeCalculator> feeCalculators;

    public FeeCalculatorResolver(Map<FeeType, FeeCalculator> feeCalculators) {
        this.feeCalculators = feeCalculators;
    }

    public double calculateFee(FeeRecord feeRecord) {
        FeeCalculator feeCalculator = feeCalculators.get(feeRecord.getFeeType());
        return feeCalculator.calculateFee(feeRecord.getPaymentAmount());
    }
}
```

The `FeeCalculatorResolver` takes in a map of `FeeType` to `FeeCalculator` and uses it to delegate to the appropriate calculator based on the `FeeRecord`'s fee type.

Finally, we'll create a `FeeCalculatorRepository` interface to retrieve the fee calculators from the database:

```
public interface FeeCalculatorRepository {
    Map<FeeType, FeeCalculator> getFeeCalculators();
}
```

The implementation of this interface would retrieve the fee calculators from the database and return them as a `Map`.

Now, to use these classes, we would first retrieve the `FeeCalculatorRepository` instance from the database and use it to create a `FeeCalculatorResolver`:

```
FeeCalculatorRepository repository = // get from database
Map<FeeType, FeeCalculator> feeCalculators = repository.getFeeCalculators();
FeeCalculatorResolver resolver = new FeeCalculatorResolver(feeCalculators);
```

Then, when we need to calculate the fee for a payment, we would create a `FeeRecord` and pass it to the resolver:

```
FeeRecord feeRecord = new FeeRecord(paymentAmount, feeType);
double fee = resolver.calculateFee(feeRecord);
```

## Rewrite it using Spring framework

## ChatGPT

Sure! Here's an updated implementation using Spring framework:

First, we'll define our `FeeCalculator` interface and implementations for fixed and percentage fees just like before:

```
public interface FeeCalculator {
    double calculateFee(double paymentAmount);
}

@Component
public class FixedFeeCalculator implements FeeCalculator {
    private static final double FIXED_FEE_AMOUNT = 2.0;

    @Override
    public double calculateFee(double paymentAmount) {
        return FIXED_FEE_AMOUNT;
    }
}

@Component
public class PercentageFeeCalculator implements FeeCalculator {
    private static final double PERCENTAGE_FEE_AMOUNT = 0.5;

    @Override
    public double calculateFee(double paymentAmount) {
        return paymentAmount * PERCENTAGE_FEE_AMOUNT;
    }
}
```

We marked the `FixedFeeCalculator` and `PercentageFeeCalculator` classes with the `@Component` annotation to let Spring know that they should be registered as beans.

Next, we'll create a `FeeRecord` class just like before:

```
public class FeeRecord {
    private double paymentAmount;
    private FeeType feeType;

    // constructor, getters and setters
}
```

`FeeType` also remains the same:

```
public enum FeeType {
    FIXED,
    PERCENTAGE
}
```

Now, we'll create a `FeeCalculatorResolver` class which will delegate to the appropriate calculator based on the fee type:

```
@Component
public class FeeCalculatorResolver {
    private final Map<FeeType, FeeCalculator> feeCalculators;

    @Autowired
    public FeeCalculatorResolver(List<FeeCalculator> feeCalculatorList) {
        feeCalculators = feeCalculatorList.stream()
                .collect(Collectors.toMap(FeeCalculator::getFeeType, Function.identity()));
    }

    public double calculateFee(FeeRecord feeRecord) {
        FeeCalculator feeCalculator = feeCalculators.get(feeRecord.getFeeType());
        return feeCalculator.calculateFee(feeRecord.getPaymentAmount());
    }
}
```

We marked the `FeeCalculatorResolver` class with `@Component` and used constructor injection with the `@Autowired` annotation to inject a `List` of all implemented `FeeCalculator` instances. We used Java 8 Stream API to convert the list to a `Map` where the key is the `FeeType` and the value is the `FeeCalculator` instance.

The `FeeCalculatorResolver` also remains the same as the previous implementation, delegating to the appropriate calculator based on the `FeeRecord`'s fee type.

Finally, we'll create a `FeeCalculatorRepository` interface that retrieves the fee calculators from a database. We'll again use constructor injection with the `@Autowired` annotation to inject a `JdbcTemplate` to perform the database queries:

```
@Repository
public class FeeCalculatorRepositoryImpl implements FeeCalculatorRepository {
    private final JdbcTemplate jdbcTemplate;

    @Autowired
    public FeeCalculatorRepositoryImpl(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    @Override
    public Map<FeeType, FeeCalculator> getFeeCalculators() {
        List<FeeCalculator> feeCalculators = jdbcTemplate.query(
            "SELECT fee_type, fee_amount FROM fee_calculators",
            (rs, rowNum) -> {
                FeeType feeType = FeeType.valueOf(rs.getString("fee_type"));
                double feeAmount = rs.getDouble("fee_amount");
                if (feeType == FeeType.FIXED) {
                    return new FixedFeeCalculator();
                } else if (feeType == FeeType.PERCENTAGE) {
                    return new PercentageFeeCalculator();
                } else {
                    return null;
                }
            }
        );

        return feeCalculators.stream()
                .collect(Collectors.toMap(FeeCalculator::getFeeType, Function.identity()));
    }
}
```

We marked the `FeeCalculatorRepositoryImpl` class with `@Repository` to let Spring know it should be registered as a bean. We used the `JdbcTemplate` to query the database and return a `List` of `FeeCalculator` instances based on the data retrieved. We again used Java 8 Stream API to convert the list to a `Map` where the key is the `FeeType` and the value is the `FeeCalculator` instance.

Now, to use these classes, we simply need to declare them as beans in a configuration class:

```
@Configuration
public class AppConfig {
    @Bean
    public FeeCalculatorResolver feeCalculatorResolver() {
        return new FeeCalculatorResolver(feeCalculators());
    }

    @Bean
    public Map<FeeType, FeeCalculator> feeCalculators() {
        Map<FeeType, FeeCalculator> feeCalculators = new HashMap<>();
        feeCalculators.put(FeeType.FIXED, new FixedFeeCalculator());
        feeCalculators.put(FeeType.PERCENTAGE, new PercentageFeeCalculator());
        return feeCalculators;
    }

    @Bean
    public FeeCalculatorRepository feeCalculatorRepository(JdbcTemplate jdbcTemplate) {
        return new FeeCalculatorRepositoryImpl(jdbcTemplate);
    }
}
```

In this configuration class, we declared the `FeeCalculatorResolver` as a bean and passed it the `feeCalculators()` bean which retrieves the `FeeCalculator` instances. We also declared the `FeeCalculatorRepository` as a bean and passed it a `JdbcTemplate` bean.

We can now use our beans to calculate fees:

```
@Bean
public CommandLineRunner run(FeeCalculatorResolver feeCalculatorResolver) throws Exception {
    return args -> {
        FeeRecord feeRecord = new FeeRecord(100.0, FeeType.PERCENTAGE);
        double fee = feeCalculatorResolver.calculateFee(feeRecord);
        System.out.println("Fee: " + fee);

        feeRecord.setFeeType(FeeType.FIXED);
        fee = feeCalculatorResolver.calculateFee(feeRecord);
        System.out.println("Fee: " + fee);
    };
}
```

In this example code, we created a `FeeRecord` with a payment amount of $100 and a `FeeType` of `PERCENTAGE`. We passed this record to the `FeeCalculatorResolver` to calculate the fee, printed the fee to the console, and then changed the `FeeType` to `FIXED` and recalculated the fee.
