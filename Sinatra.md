Sinatra
=======

---

Sinatra
-------

* DSL for web applications
* micro framework
    - provide structure and helper methods
    - free choice of templates, data access and architecture (MVC, MVP, etc.)

---

Routes and parameters (1)
-------------------------

    !ruby
    get '/login' do
      haml :login
    end

    # get('/login', { haml :login})

    post '/login' do
      if not User.check_login(params[:login],
                          params[:password])
        redirect '/login'
      end
      # Register user session
      redirect '/'
    end

---

Routes and parameters (2)
-------------------------

    !ruby
    get '/user/:login/edit' do |login|
      @user = get_user(["administrator"])
      @app_user = User[login]
      if @user.nil? or @app_user.nil?
        raise Sinatra::NotFound
      end
      haml :admin_user
    end

    get %r{^/(t1|t2)/([\d]+)$} do |type,id|
      # DRY, same function for similar code
      # /t1/10 : type = t1, id = 10
      # /t2/25 : type = t2, id = 25
    end

---

Filters
-------

    !ruby
    # Execute authorization check before
    # each request, except for web service
    # and login page
    before %r{^(?!(/login|/ws/).*)} do
      authorize!
    end

    after do
      # Log request status
    end

---

Content types and status codes
------------------------------

    !ruby
    get %r{^/ws/agent/([\d]+).json$} do |id|
        content_type :json
    
        patient = Patient[id]
        if patient.nil?
          halt 404, "Patient[#{id}]not found"
        end
    
        {
            "patient_id" => patient.id,
            "agent_mind" =>
              patient.agent.cognitive_model
        }.to_json
    end

---

Templates
---------

* Free choice of template engine
* I use haml

        !ruby
        get '/login' do
          haml :login
        end

---

HAML
----

    !haml
    !!! 5
    %html
      %head
        %meta{:charset => "utf-8"}
        %title Login
      %body
        %h1 G-DEMANDE
        %form{:action => "/login"}
          %div.loginform
            %label{:for => "login"} Login:
            %input#login{:name => "login"}
        - ny = "01.01.2012"
        - if Date.today = Date.parse(ny)
          %div Happy new year!
