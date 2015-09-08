# Introduction to Testing, BDD, and RSpec

### Pairing Discussion
- What went well?
- What was difficult?
- Checking in with your pair
- Giving and receiving feedback

### Why Do We Care About Testing?
- Programatically specify behavior
- Ensure program correctness
- Cut down debugging time
- Enable refactoring
- Reduce unnecessary code, overdesigning, feature creep

### What is a Spec?
- A description of expected behavior that we can run against production code

### What is TDD/BDD?
- TDD/BDD means we write our spec code before we write production code
- Testing a whole code base after it is written is incredibly challenging
- Red, Green, Refactor cycle
- We do just enough to make the tests pass, which disciplines us to write code in small, functional pieces
- The goal is the test
- With BDD we test behavior, which means focusing on business value. TDD focuses on how something will work, BDD focuses on why we build it at all.  Tools like RSpec (and Cucumber and Capybara, which you will learn about later) are helping push the industry in this direction.

### Setting up Rspec
- RSpec is a gem
- RSpec is a DSL for testing (as opposed to a GPL like ruby)
- A DSL is specific to a problem domain

First, we install the gem.

```
gem install rspec
```

Then we create a spec file, `calendar_spec.rb`, and require the code under test within that file.

```ruby
require_relative 'calendar'
```

This loads the code in `calendar.rb`.  The code is loaded exactly once, so if we were to require the same file again, it would not be loaded again.  Do you know what the difference between `require` and `require_relative` is?  `require_relative` allows you to "load a file that is relative to the file containing the require_relative statement". With `require`, `./` indicates a path that is relative to your current working directory.

Then we can run our spec with the following command.

```
rspec calendar_spec.rb 
```

We fail with an error.  Our `calendar.rb` file doesn't exist yet.  Let's create an empty file, just enough to pass the spec.

Now let's break our tests and specify that we want to describe a calendar component.  We can add the following to our `calendar_spec.rb`

```ruby
require_relative 'calendar'

describe Calendar do
  before :each do
    @calendar = Calendar.new
  end
end
```

Now the tests fail again.  Let's make them work by modifying `calendar.rb`

```ruby
class Calendar

end
```

Okay, let's start defining some calendar behavior in our `calendar_spec.rb`

```ruby
require_relative 'calendar'
require 'date'

describe Calendar do
  before :each do
    @calendar = Calendar.new
  end

  it "should parse a D M Y format string into a date" do
    date_string = "10 9 2015"
    expect(@calendar.parse_date(date_string)).to eq Date.new(2015, 9, 10)
  end
end
```

And let's write the code to pass the test in `calendar.rb`

```ruby
class Calendar

  def parse_date date_string
    day = date_string.split(" ")[0].to_i
    month = date_string.split(" ")[1].to_i
    year = date_string.split(" ")[2].to_i
    Date.new(year, month, day)
  end
end
```

Now that we have an exmaple going, if we run the `rspec` command with `--format doc` flag, we'll get documentation format output from our test run.
```
rspec calendar_spec.rb --format doc
```

### RSpec terminology and syntax
- `describe`: The `describe` method creates an example group.  We're calling a method defined in the rspec gem.  We pass in the name of the component under test and a block that has the examples.
- `before`: A method that (optionally) takes a symbol and a block.  The symbol specifies when to run the block.  This is used for set up that is shared between tests, to DRY up your specs.
- `it`: each example is defined with the `it` method.  `it` takes a string that describes the requirement, which reads like an English sentence, and a block that contains the test case.
- `expect`: expectations are `should` and `should_not` and work together with `matchers` to express an outcome that should happen
- `matchers`: built-in matchers that allow you to test
   - Equivalence 
    ```expect(actual).to eq(expected)`
   - Identity 
    `expect(actual).to be(expected)`
   - Comparisons `expect(actual).to be >= expected` 
   - Types `expect(actual).to be_an_instance_of(expected)`
   - Booleans `expect(actual).to be_truthy`
   - Regular expressions `expect(actual).to match(/expression/)`
   - Errors `expect { ... }.to raise_error`
   - And so much more!  Read about it in the resource linked below.
- `context`: 
- `let`

### Testing best practices
- Test one thing at a time
- Keep tests isolated and independent from one another

### Resources
- [require and require relative ruby docs](http://ruby-doc.org/core-2.1.2/Kernel.html)
- [rspec gem documentation](http://rspec.info/documentation/)
- [Starter BDD example, from the rspec docs](http://rspec.info/documentation/3.3/rspec-core/#Get_Started)
- [RSpec expectation , from the rspec docs)(https://github.com/rspec/rspec-expectations)
