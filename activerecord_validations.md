<h1>DBC Phase 2 Reference Sheet:<br/>
Database Validations for ActiveRecord</h1>

<h4>Prepared by Jeff George for future DBC students</h4>

Do not let invalid data into your database! Use all possible validations at all possible levels! These levels are:
```
HTML      Form input types and attributes
Models    Validations (including custom validations)
Database  Constraints (added during migrations)
```

### HTML form input types and attributes
HTML form validations are easy to implement, and provide neatly-formatted, easily-understood messages when users make errors entering data into forms. They are easily circumvented through Chrome or Firefox Dev Tools, though, so don't count on them as security. (They keep your mom from entering a 2-letter password, but won't protect against real hackers.) There are many more input types and attributes in HTML5 than are listed here, but these are the ones I found useful in Phase 2 at DBC. Google "HTML input types" and "HTML input attributes" for code examples and complete instructions on their use.

**_Word to the wise:_** If you implement HTML form validations early in your development process, they will interfere with your testing of ActiveRecord model validations and migration constraints. For example, if you put a minimum length of 6 characters on usernames in the HTML for your new user form, it will catch too-short usernames before they ever reach your model validation for username length, so that your model validations don't get properly tested. For this reason, either add your HTML form validations _after_ you've thoroughly tested your ActiveRecord validations, or deliberately disable them to allow your AR validations to be tested, then the HTML validations back on.
```
INPUT TYPES__________________________________________________________
  text
  password
  email
  url
  number
  date

INPUT ATTRIBUTES_____________________________________________________
  For any field:
    required
  For strings:
    maxlength
    minlength
  For numbers:
    max
    min

Useful attributes that are not technically validations...............
  disabled
  placeholder
  size (width in characters)
  autofocus (places cursor in field)
  value (sets default value in field; useful in /edit forms)

INPUT FIELD EXAMPLE__________________________________________________

  <input type='text' name='username' id='username_input' placeholder='Username'
    minlength=4 maxlength=20 required />

```

### ActiveRecord Model Validations
```ruby
null: false,
presence: true,
length: { in: 4..20 },
uniqueness: true

# Cheesy, low-rent URL validation:
validates :URL          presence: true,
                        format: /(http|https):\/\//
```


#### Custom Model Validations
**_Note:_** You must **add an error message** to the errors array to make custom validations work. Custom validations **do not** return true/false. Also note that the syntax is _"validate"_, not _"validate**s**"_.

```ruby
class Thing < ActiveRecord::Base
  validate    :start_before_end

  private

  def start_before_end
    unless self.start < self.end
      self.errors << "It has to start before it ends!"
    end
  end
end
```

### ActiveRecord Database Constraints
Typical fields and the constraints to apply to them in migrations.
```ruby
# For almost all fields:
t.string          :field_name, null: false

# For usernames and other fields that need to be unique:
t.string          :username, null: false, unique: true

# For text strings with maximum lengths, such as usernames:
t.string          limit: 20
# (There is no way to set a minimum string length as a database constraint.)

# For foreign keys:
t.integer         :field_name_id, null: false, index: true
# (Consider indexing any field that gets searched more often than it gets updated.)

# For money:
t.decimal         :price, precision: 8, scale: 2
# (This handles values from -999,999.99 to 999,999.99;
# if you need to handle larger values, increase the value set for precision.)
```


