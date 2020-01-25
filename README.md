PROJECT PREP AFTER CLONE        rake db:create
                                rake db:migrate
                                rake db:seed


MAKE A NEW RAILS PROJECT        rails new rails_record_store  -d postgresql -T
ADD GEMS                        gem 'jquery-rails'

                                group :development, :test do

                                group :development, :test do
                                gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
                                gem 'rspec-rails'
                                gem 'launchy'
                                gem 'pry'
                                end

                                group :development do

SET UP rspec                    bundle exec rails generate rspec:install

SET UP DATABASE                 rake db:create

ADD TABLE                       rails generate migration create_THING

                                EDIT MIGRATION IN DB/MIGRATE/THING
                                EX:  class CreateAlbums < ActiveRecord::Migration[5.2]
                                        def change
                                          create_table(:albums) do |t|
                                          t.column(:name, :string)
                                          t.column(:year, :integer)

                                          t.timestamps()
                                          end
                                        end
                                      end

                                GOOD PRACTICE TO USE TIMESTAMPS

MIGRATE TABLE                   rake db:migrate

ADD COLUMN TO TABLE             rails g migration add_THING_to_TABLEs
                                MIGRATE TABLE

REMOVE COLUMN FROM TABLE        remove_column(:TABLEs, :THING)
                                MIGRATE TABLE

UNDO Migration                  rake db:rollback
                                ONLY BEFORE COMMIT OR PUSH

DELETE GIT BRANCH               // delete branch locally
                                git branch -d localBranchName  OR -D

                                // delete branch remotely
                                git push origin --delete REMOTEBRANCHNAME

GIT MERGE                       ensure merging branch has been pushed
                                git checkout master
                                git merge BRANCHNAME

VALIDATIONS                     spec/models/album_spec.rb
                                describe(Album) do
                                  ...
                                  it { should validate_length_of(:name).is_at_most(100) }
                                end  

                                app/models/album.rb
                                class Album < ApplicationRecord
                                  ...
                                  validates_length_of :name, maximum: 100
                                end

RAILS CONSOLE                   rails c

NEW DATABASE ENTRY              2.2.0 :001 > album = Album.new
                                => #<Album id: nil, name: nil, year: nil, created_at: nil, updated_at: nil, genre: nil>  

ASSIGN VALUE                    2.2.0 :002 > album.name="In Rainbows"
                                => "In Rainbows"
                                2.2.0 :003 > album
                                => #<Album id: nil, name: "In Rainbows", year: nil, created_at: nil, updated_at: nil, genre: nil>

SAVE                            2.2.0 :004 > album.save
                                (0.2ms)  BEGIN
                                Album Create (0.4ms)  INSERT INTO "albums" ("name", "created_at", "updated_at") VALUES ($1, $2, $3) RETURNING "id"  [["name", "In Rainbows"], ["created_at", "2019-06-17 15:46:54.422926"], ["updated_at", "2019-06-17 15:46:54.422926"]]
                                (0.8ms)  COMMIT
                                => true                                                                   

LOOK AT ENTRY                   2.2.0 :005 > album
                                => #<Album id: 1, name: "In Rainbows", year: nil, created_at: "2019-06-17 15:46:54", updated_at: "2019-06-17 15:46:54", genre: nil>                      

CREATE(NEW AND SAVE)            Album.create(name: "Giant Steps")

                                 (0.2ms)  BEGIN
                                Album Create (0.3ms)  INSERT INTO "albums" ("name", "created_at", "updated_at") VALUES ($1, $2, $3) RETURNING "id"  [["name", "Giant Steps"], ["created_at", "2019-06-17 15:58:20.395727"], ["updated_at", "2019-06-17 15:58:20.395727"]]
                                 (0.7ms)  COMMIT
                                 => #<Album id: 2, name: "Giant Steps", year: nil, created_at: "2019-06-17 15:58:20", updated_at: "2019-06-17 15:58:20", genre: nil>


                                 IF ISSUES, PUT A !. GET MORE DESCRIPTIVE ERROR MESSAGE

                                 012:0> Song.create!(name: "Reckoner")
                                     (0.2ms)  BEGIN
                                     (0.2ms)  ROLLBACK
                                  ActiveRecord::RecordInvalid: Validation failed: Album must exist
                                      from (irb):12
