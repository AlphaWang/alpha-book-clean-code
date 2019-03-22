# 8 Boundaries

## My Summary
    
    1. Boundaries is the boundary between third party codes and our codes. 
    2. Third party codes always includes so many functions, but sometimes we only use several of them. That's why we need to make the boudaries clear.
    3. Encapsulation is the way to keep that boundary clear.
    4. Main purpose is to reduce the workload when things need to change.

## Intros of this Chapter
### 8.1 Using Third Party Codes

Third-party codes are easy to use. So we like to use them.
    
#### For example

    We use many api( backend things, I'm not so familiar) provided by other teams and Utils.
    We use node.js, webpack, lodash, iScroll or Swiper and etc.
    It's hard to calculate how many third-party-codes we use, cause it's too many.
    
#### The author use java.util.map as example.
    
Backgroud:

    To create Map with a specific class named Sensor.
    Positive points: java.util.map includes all basic functions of map
    Negetive points: java.util.map can accept all data.
    
Bad Codes:
    
    Map sensors = new HashMap()
    
Why is bad codes? To see how to use it:

    first way:
    Sensor s = (Sensor)sensor.get(sensorId)
    
    second way, a little better:
    Map<Sensor> sensors = new HashMap<Sensor>();
    ...
    Sensor s = sensors.get(sensorId)
    
So, if java.util.map's api changed, perhaps get() changed to getById(), we need to search all project to change sensors.get() to sensors.getById().

Good codes:

    public class Sensors {
        private Map sensors = new HashMap();
        
        public Sensor get(Strng id) {
            return (Sensor) sensors.get(id);
        }
        
        ...
        
    }
    
Recommendation from Author
    
    We are not suggesting that every use of Mapbe encapsulated in this form. Rather, 
    weare  advising  you  not  to  pass  Maps  (or  any  other  interface  at  a  boundary)  around  yoursystem. 
    If you use a boundary interface like Map, keep it inside the class, or close familyof  classes,  where  it  is  used.  
    Avoid  returning  it  from,  or  accepting  it  as  an  argument  to,public APIs.
    
### 8.2 Exploring and Learning Boundaries
### 8.3 Learning log4j

#### Recommendation: Writing test cases to learning the usage of third-party codes.

Use log4j for exmaple. To summary, try and try until the test is passed.

Background:

    Use log4j to writing console.
    
Codes:

    first try, have issue, so check the doc ...
    @Testpublic void testLogCreate() {
        Logger logger = Logger.getLogger("MyLogger");
        logger.info("hello");
    }
    
    second try, failed, check the doc ...
    @Testpublic void testLogAddAppender() {
        Logger logger = Logger.getLogger("MyLogger");
        ConsoleAppender appender = new ConsoleAppender();
        logger.addAppender(appender);
        logger.info("hello");
    }
    
Finnally, third try will pass. But ... the code is too long ... so I will not paste here ...
#### So, why not we just check the doc and write the codes? why use test case to learning? Is it because the author is stupid?

### 8.4 Learning Test are Better than Free

#### 1 improve our understanding of the lib
#### 2 could just run test case to verify any update of lib's new version on the functions we using now

I think NO.1 is nonsense, there are many ways to understanding the lib. But the NO.2 seems very awesome.

### 8.5 Using Codes That Does Not Exist Yet

I'm not sure this part have some relationship to the third-party codes.
Mostly, I think ... emmmm ... this part is about api design ...

Author's example:

    They developed a system with a sub-system called Transimtter. No one knows about that Transimitter.
    They only know, input is frequency and the output is a analog presentation.
    
    so, they decided to not implement the details but only make the api structure design.
    
Emmm ... I think it's 

### 







