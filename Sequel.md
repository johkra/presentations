Sequel
======

---

Sequel
------

* SQL DSL for Ruby
    - method chaining
* ORM
* DB abstraction
    - Switch from Mysql to Postgres
    - Only change connection string

---

Ruby SQL DSL
------------

    !ruby
    # Print time and message of 5
    # most severe alerts for a patient
    DB[:alerts].
    join(:agents, :id => :agent_id).
    join(:patients, :id => :patient_id).
    filter(:patients__surname => "Svenson").
    order(:severity).
    limit(5).
    select(:time, :alert => :message).
    each do |row|
      puts "#{row[:time]}: #{row[:message]}"
    end

---

ORM (1)
-------

    !ruby
    class Language < Sequel::Model
    end

    Language.create(:language => "xx")
    => #<Language {:language=>"xx", :id=>18}>

    Language[18].language
    => "xx"

    Language[18].update(:language => "yz")
    => #<Language {:language=>"yz", :id=>18}>

    Language[18].delete

---

ORM (2)
-------

    !ruby
    class Patient < Sequel::Model
      one_to_one :agent, :key patient_id
      many_to_many :languages
    end

    pilar = Patient[1]
    pilar.languages.map{|x| x.language}
    => ["en", "es", "fr"]

    de = Language.filter(:language => "de")
    pilar.add_language( de.first )

    pilar.agent.observations.count
    => 2

---

ORM (3)
-------

    !ruby
    class Patient < Sequel::Model
      def age()
        # Calculate age
      end
    
      def bmi()
        # Calculate BMI
      end
    end

    pilar.age
    => 27
    pilar.bmi
    => 23.2

