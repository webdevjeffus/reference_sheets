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
```

### ActiveRecord Model Validations
```ruby
null: false,
presence: true,
length: { in: 4..20 },
uniqueness: true

Cheesy, low-rent URL validation:
validates :URL          presence: true,
                        format: /(http|https):\/\//
```


#### Custom Model Validations
_Note: You must *add an error message* to the errors array to make custom validations work. Custom validations **do not** return true/false. Note that the syntax is "validate", not "validates"._

```ruby
class Thing < ActiveRecord::Base
  validate    :start_before_end

  private

  def start_before_end
    unless self.start < self.end
      self.errors << "It doesn't start until after it ends!"
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

# For text strings with maximum lengths
# (I haven't found a minimum limit yet...)
t.string          :limit: 20

# For money:
t.field_name      :decimal, precision: 8, scale: 2
scale:

# For foreign keys:
# (Consider any indexing any field that gets searched alot)
t.integer         :field_name_id, null: false, index: true
```


