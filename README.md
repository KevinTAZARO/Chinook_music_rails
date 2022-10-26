# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...
Questions
Maintenant que ta BDD est prête, tu vas répondre aux questions ci-dessous :

a) Niveau facile
Quel est le nombre total d'objets Album contenus dans la base (sans regarder les id bien sûr) ?
==>Album.count
  TRANSACTION (0.0ms)  begin transaction
  Album Count (0.2ms)  SELECT COUNT(*) FROM "albums"
 => 347

Qui est l'auteur de la chanson "White Room" ?
==>Track.find_by(title:"White Room").artist
  Track Load (0.5ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."title" = ? LIMIT ?  [["title", "White Room"], ["LIMIT", 1]]
 => "Eric Clapton" 

Quelle chanson dure exactement 188133 milliseconds ?
==>Track.find_by(duration:188133).title
  Track Load (0.2ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."duration" = ? LIMIT ?  [["duration", 188133], ["LIMIT", 1]]
 => "Perfect"

Quel groupe a sorti l'album "Use Your Illusion II" ?
==>Album.find_by(title:"Use Your Illusion II").artist
  Album Load (2.2ms)  SELECT "albums".* FROM "albums" WHERE "albums"."title" = ? LIMIT ?  [["title", "Use Your Illusion II"], ["LIMIT", 1]]
 => "Guns N Roses"

b) Niveau Moyen
Combien y a t'il d'albums dont le titre contient "Great" ? (indice)
==> Album.where("title like ?", "%Great%").count
  Album Count (0.4ms)  SELECT COUNT(*) FROM "albums" WHERE (title like '%Great%')
 => 13

Supprime tous les albums dont le nom contient "music".
==>Album.where("title like ?", "%music%").destroy_all
  Album Load (0.2ms)  SELECT "albums".* FROM "albums" WHERE (title like '%music%')
  TRANSACTION (0.2ms)  SAVEPOINT active_record_1
  Album Destroy (1.7ms)  DELETE FROM "albums" WHERE "albums"."id" = ?  [["id", 315]]
  TRANSACTION (0.1ms)  RELEASE SAVEPOINT active_record_1
  TRANSACTION (0.1ms)  SAVEPOINT active_record_1
  Album Destroy (0.2ms)  DELETE FROM "albums" WHERE "albums"."id" = ?  [["id", 319]]
  TRANSACTION (0.1ms)  RELEASE SAVEPOINT active_record_1
  TRANSACTION (0.1ms)  SAVEPOINT active_record_1
  Album Destroy (0.2ms)  DELETE FROM "albums" WHERE "albums"."id" = ?  [["id", 333]]
  TRANSACTION (0.1ms)  RELEASE SAVEPOINT active_record_1
  TRANSACTION (0.1ms)  SAVEPOINT active_record_1
  Album Destroy (0.2ms)  DELETE FROM "albums" WHERE "albums"."id" = ?  [["id", 346]]
  TRANSACTION (0.1ms)  RELEASE SAVEPOINT active_record_1
 =>
[#<Album:0x00007f75a0887bc0
  id: 315,
  title: "Handel: Music for the Royal Fireworks (Original Version 1749)",
  artist: "English Concert & Trevor Pinnock",
  created_at: Wed, 26 Oct 2022 16:13:27.451043000 UTC +00:00,
  updated_at: Wed, 26 Oct 2022 16:13:27.451043000 UTC +00:00>,
 #<Album:0x00007f75a0887af8
  id: 319,
  title: "Armada: Music from the Courts of England and Spain",
  artist: "Fretwork",
  created_at: Wed, 26 Oct 2022 16:13:27.478223000 UTC +00:00,
3.0.0 :011 > Album.count
  Album Count (0.1ms)  SELECT COUNT(*) FROM "albums"
 => 343

Combien y a t'il d'albums écrits par AC/DC ?
==>Album.where(artist:"AC/DC").count
  Album Count (0.2ms)  SELECT COUNT(*) FROM "albums" WHERE "albums"."artist" = ?  [["artist", "AC/DC"]]
 => 2

Combien de chanson durent exactement 158589 millisecondes ?
==>Track.where(duration:158589).count
  Track Count (0.3ms)  SELECT COUNT(*) FROM "tracks" WHERE "tracks"."duration" = ?  [["duration", 158589]]
 => 0

c) Niveau Difficile
Pour ces questions, tu vas devoir effectuer des boucles dans la console Rails. C'est peu commun mais c'est faisable, tout comme dans IRB.

puts en console tous les titres de AC/DC.
==>Track.where(artist:"AC/DC").each do |x|
3.0.0 :016 >   puts x.title
3.0.0 :017 > end
  Track Load (0.2ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."artist" = ?  [["artist", "AC/DC"]]
For Those About To Rock (We Salute You)
Put The Finger On You
Lets Get It Up
Inject The Venom
Snowballed
Evil Walks
C.O.D.
Breaking The Rules
Night Of The Long Knives
Spellbound
Go Down
Dog Eat Dog
Let There Be Rock
Bad Boy Boogie
Problem Child
Overdose
Hell Aint A Bad Place To Be
Whole Lotta Rosie

puts en console tous les titres de l'album "Let There Be Rock".
==>Track.where(album:"Let There Be Rock").each do |y|
3.0.0 :022 >   puts y.title
3.0.0 :023 > end
  Track Load (0.2ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."album" = ?  [["album", "Let There Be Rock"]]
Go Down
Dog Eat Dog
Let There Be Rock
Bad Boy Boogie
Problem Child
Overdose
Hell Aint A Bad Place To Be
Whole Lotta Rosie

Calcule le prix total de cet album ainsi que sa durée totale.
==>3.0.0 :038 > Track.where(album:"Let There Be Rock").each do |y|
3.0.0 :039 >   album_price += y.price
3.0.0 :040 >   album_duration += y.duration
3.0.0 :041 > end
  Track Load (0.2ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."album" = ?  [["album", "Let There Be Rock"]]
 =>  duration: 254380,
  size: 8331286,
3.0.0 :043 > album_price
 => 7.920000000000001
3.0.0 :044 > album_duration/60/1000
 => 40

Calcule le coût de l'intégralité de la discographie de "Deep Purple".
==>3.0.0 :046 > Track.where(artist:"Deep Purple").each do |t|
3.0.0 :047 >   disco_price += t.price
3.0.0 :048 > end
  Track Load (0.4ms)  SELECT "tracks".* FROM "tracks" WHERE "tracks"."artist" = ?  [["artist", "Deep Purple"]]
 => 3.0.0 :049 > disco_price
 => 90.0899999999999

Modifie (via une boucle) tous les titres de "Eric Clapton" afin qu'ils soient affichés avec "Britney Spears" en artist.
==>  Track.where(artist:"Eric Clapton").each do |r|
3.0.0 :051 >   r.update(artist:"Britney Spears")
3.0.0 :052 > end