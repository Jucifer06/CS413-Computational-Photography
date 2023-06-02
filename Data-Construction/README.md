Please find the data folder at this GoogleDocs link: "https://drive.google.com/drive/folders/1fvL8H6vavCEcVUXwqrLawLeYp2dv5UYY?usp=sharing"

# Explain the dataset construction process

## EXP3:

First real dataset creation after some previous experiments.

Both nature and city scenaries, with an object in the foreground. We tried to add some weather/daytime descrption too.

882 image pairs

only AttentionRefine

	objects = ["bench", "street lamp", "castle", "trashcan", "hair salon", "pizza truck", "fountain"] 
	colors = ["green", "blue", "black"] 
	backgrounds = ["a shopping street", "New York", "Paris", "a forest", "the ocean", "mountains", "London"] 
	weathers = ["nigthtime", "daytime", "dawn", "raining", "sunny", "snowing", "clouded"] 
	seeds = [1001] 

	base_text =  "A real-life photograph of a " + c + " " + o + ", with " + b + " in the background. It is " + w
	old_toned_text = ". The scene is a bleached washed-out colored post-apocalyptic photograph, it has been abandoned for centuries. The buildings and background are dirty and decaying. The objects are rusty and trashed. The vegetation is dry and dead."
	mod_text = base_text + old_toned_text
    
	"cross_replace_steps: 0.1 |self_replace_steps: 0.9 |num_steps: 50 |guidance_scale: 7.5 |controller: AttentionRefine |stable: ldm_stable"


## EXP4:

Big dataset ceration of various interiors

First try with human transformation

2340 image pairs

only AttentionRefine

	interiors = ["a doctor office", "a flower shop", "a church","a temple", "a mosque", "a synagogue", "a living room", "a bedroom", "a bathroom", "a kitchen", "a garage", "a cellar", "a hospital","a mall", "a bar","a coffee shop", "a restaurant", "a train station", "a hall", "a supermarket", "a bank", "a library", "a university", "a research lab", "a makeup store", "a museum"]                        
	colors = ["navy blue", "pink", "dark green", "grey", "green", "blue", "black", "red", "orange", "purple", "yellow", "brown", "cyan", "leather", "metallic"] 
	attendances = ["crowded", "empty"]
	seeds = [1001, 1003, 1004]

	 base_text =  "A real-life photograph of the " + a + " interior of " + interior +" with " + c + " furniture.
	old_toned_text = " The scene is a bleached washed-out colored post-apocalyptic photograph, it has been abandoned for centuries. The room is empty and there is nobody. The interior and furniture are dirty and decaying. The objects are rusty and trashed. The vegetation is dry and dead."
	mod_text = base_text + old_toned_text

	"cross_replace_steps: 0.1 |self_replace_steps: 0.9 |num_steps: 50 |guidance_scale: 7.5 |controller: AttentionRefine |stable: ldm_stable"
    

## EXP5:

more images focused on cities, with objects in the foreground, with/without humans
 
1008 image pairs

only AttentionRefine

	objects = ["metallic trash can","street lamp", "car","bus"]                        
	colors = ["navy blue", "dark green", "red", "purple", "yellow", "brown"] 
	backgrounds = ["New York","London","Paris","Bangkok","Honk Kong","Macao","Dubai","Singapore","Delhi","Istanbul","Rome","Mumbai","Tokio","Prague","Moscow","Barcelona","Los Angeles","Las Vegas","Cairo","Berlin","Jaipur"]
	attendances = ["crowded", "empty"]
	seeds = [1003]

	base_text =  "A real-life photograph of a " + c + " " + o + " in the "+a+" streets of " + b +"."
	old_toned_text = "The scene is a bleached washed-out colored post-apocalyptic photograph, it has been abandoned for centuries. The city and buildings are dirty and decaying. The objects are rusty and trashed. The vegetation is dry and dead."
      mod_text = base_text + old_toned_text

	"cross_replace_steps: 0.1 |self_replace_steps: 0.9 |num_steps: 50 |guidance_scale: 7.5 |controller: AttentionRefine |stable: ldm_stable"


## EXP6:

First try of AttentionReweight

First try with text in the image: often some incoherent words

504 images

use Attention reweight on ("abandoned","dirty","rusty","uncrowded","post-apocalyptic","bleached","empty","tattered","peeled") with a factor of 10

	objects = ["advertisement billboard","banner ads","wallscapes","graffiti"]  
	backgrounds = ["New York","London","Paris","Bangkok","Honk Kong","Macao","Dubai","Singapore","Delhi","Istanbul","Rome","Mumbai","Tokio","Prague","Moscow","Barcelona","Los Angeles","Las Vegas","Cairo","Berlin","Jaipur"]
	attendances = ["crowded", "empty"]
	seeds = [983497,87432, 3003]

	base_text =  "The scene is a lifelike color modern photograph showing the "+a+" city of "+b+", with a " + o +" in the background."
	old_toned_text = " The scene is a bleached colored, uncrowded, deserted, abandonned post-apocalyptic photograph. The buildings are dirty, trashed, rusty. The billboard is peeled, torn appart and tattered."
      mod_text = base_text + old_toned_text

	"cross_replace_steps: 0.1 |self_replace_steps: 0.9 |num_steps: 150 |guidance_scale: 7.5 |controller: AttentionReweight |stable: ldm_stable"


## EXP7:

54 images

Some images that we missed on the previous dataset creations.

use Attention reweight on ("abandoned","dirty","rusty","uncrowded","post-apocalyptic","bleached","dry","dead")

	objects = ["a carnival", "an attraction park","a plane on the ground","a train","a fishing boat","a sailing boat","a yatch","a medieval castle", "a chest", "a palace", "the library of congress", "the Bodleian library", "the Vatican","the interior of the Vatican","the library of the Vatican", "the Boston public library", "the Thomas Fisher rare book library","the Seattle Central library"]  
	seeds = [4000,983497, 87432]

	base_text =  "The scene is a realistic lifelike color modern photograph showing "+o+"."
	old_toned_text = " The scene is a bleached colored, uncrowded, deserted, abandonned post-apocalyptic photograph. The buildings are dirty, trashed, rusty. The objects are rusty and trashed. The vegetation is dry and dead."
	mod_text = base_text + old_toned_text

	"cross_replace_steps: 0.1 |self_replace_steps: 0.9 |num_steps: 75 |guidance_scale: 7.5 |controller: AttentionReweight |stable: ldm_stable"

## EXP8:

28 image pairs

Experiment of reverse generation: going from abandoned images back to the oiginal, thanks to the AttentionReweight

Abandoned images look great, but the original images are extremely unstable and in general not usable at all -> failure

AttentionReweight on ("abandoned","deserted","post-apocalyptic") with factor of -100

	objects = ["a flower shop", "a living room", "a castle","a library", "a coffee shop", "a bar"]  
	seeds = [21984,987432]

	base_text =  "The scene is abandonned, deserted, post-apocalyptic photograph. The scene shows the interior of "+o+"."
	mod_text = base_text

	"cross_replace_steps: 0.1 |self_replace_steps: 0.9 |num_steps: 150 |guidance_scale: 7.5 |controller: AttentionReweight |stable: ldm_stable"


## EXP9:

29 image pairs

Real cities

AttentionReweight on ("abandoned","dirty","rusty","uncrowded","post-apocalyptic","bleached","dry","dead") with a factor of 20

	cities = ["Rio De Janeiro", "Marrakesh","Kyoto", "Salvador", "Mexico", "Buenos Aires", "Cape Town","Las Vegas","Paris","Bangkok","Dubai","Macao","Barcelona", "Madrid", "Amsterdam", "New York", "Lima", "Turin", "Geneva", "Zurich", "Lausanne", "Bern", "London", "Roma", "New Orleans", "Samarkand", "Moscow", "Boston", "Washington DC"]
	seeds = [4000]

	base_text =  "The scene is a realistic lifelike color modern photograph showing the city of "+c+"."	
	old_toned_text = " The scene is a bleached colored, uncrowded, deserted, abandonned post-apocalyptic photograph. The buildings are dirty, trashed, rusty. The objects are rusty and trashed. The vegetation is dry and dead."
	mod_text = base_text + old_toned_text

	"cross_replace_steps: 0.1 |self_replace_steps: 0.9 |num_steps: 75 |guidance_scale: 7.5 |controller: AttentionReweight |stable: ldm_stable"


## EXP10:

Only cars in urban and landscape settings

Start of the creation of the lastly used trainset for our Pix2Pix model

Change the dataset creation process: concentrate only on car (and later street) scenes generation. So the descrption stays mostly the same, with some color changes, and 
more importantly seed changes (the scene can be described the same, but changing the seed will change the style, the surroundings,...).

AttentionReweight on  ("abandoned","dirty","rusty","uncrowded","post-apocalyptic","bleached","dry","dead") with a factor of 4

800 image pairs

	colors = ["blue", "orange","red","green","yellow"]
	vehicules = ["car"]
	backgrounds = ["a city","the ocean", "fields","the montains"]
	seeds = list(range(4000,4040))

	base_text =  "The scene is a realistic photograph showing a "+c+" "+v+" with "+b+" in the background."
	old_toned_text = " The scene is a bleached colored, uncrowded, deserted, abandonned post-apocalyptic photograph. The background is dirty, trashed, rusty. The vehicule is rusty and trashed. The vegetation is dry and dead."
	mod_text = base_text + old_toned_text

	"cross_replace_steps: 0.1 |self_replace_steps: 0.9 |num_steps: 75 |guidance_scale: 7.5 |controller: AttentionReweight |stable: ldm_stable"


## EXP11:

Only cars in urban setting

Continue the creation of the lastly used trainset for our Pix2Pix model

AttentionReweight on ("destroyed", "abandoned","dirty","rusty","uncrowded","post-apocalyptic","bleached","dry","dead") with a 10 factor

2000 image pairs

	colors = ["blue", "orange","red","green","yellow"]
	vehicules = ["car"]
	backgrounds = ["a city","a street"]
	seeds = list(range(780000,780200))

	base_text =  "The scene is a realistic photograph showing a "+c+" "+v+" with "+b+" in the background."
	old_toned_text = " The scene is a bleached colored, uncrowded, deserted, abandonned post-apocalyptic photograph. The buildings are destroyed and trashed. The car is rusty and trashed. The vegetation is dry and dead."
	mod_text = base_text + old_toned_text

	"cross_replace_steps: 0.1 |self_replace_steps: 0.9 |num_steps: 75 |guidance_scale: 7.5 |controller: AttentionReweight |stable: ldm_stable"


## EXP12:

Ony cars in urban setting

Continue the creation of the lastly used trainset for our Pix2Pix model

AttentionReweight on ("destroyed", "abandoned","dirty","rusty","uncrowded","post-apocalyptic","bleached","dry","dead") with a factor of 10

2000 image pairs

	colors = ["blue", "orange","red","green","yellow"]
	vehicules = ["car"]
	backgrounds = ["a city","a street"]
	seeds = list(range(59000,59200))

	base_text =  "The scene is a realistic photograph showing a "+c+" "+v+" with "+b+" in the background."
	old_toned_text = " The scene is a bleached colored, uncrowded, deserted, abandonned post-apocalyptic photograph. The buildings are destroyed and trashed. The car is rusty and trashed. The vegetation is dry and dead."
	mod_text = base_text + old_toned_text

	"cross_replace_steps: 0.1 |self_replace_steps: 0.9 |num_steps: 75 |guidance_scale: 7.5 |controller: AttentionReweight |stable: ldm_stable"


## EXP13:

Only cities and villages, with and without people

AttentionReweight on ("destroyed", "abandoned","dirty","rusty","uncrowded","post-apocalyptic","bleached","dry","dead") with a factor of 10

957 image pairs

	attendances = ["an uncrowded"]
	objects = ["street", "city", "village"]
	seeds = list(range(1, 320))

	base_text =  "The scene shows "+a+" "+o
	old_toned_text = ". The setting is bleached-colored, uncrowded, deserted, abandoned post-apocalyptic. The buildings are destroyed, rusty, and trashed. The vegetation is dry and dead."
	mod_text = base_text + old_toned_text

	"cross_replace_steps: 0.1 |self_replace_steps: 0.9 |num_steps: 75 |guidance_scale: 7.5 |controller: AttentionReweight |stable: ldm_stable"


## EXP14:

More image of urban settings, but never incorporated in the training dataset (came at a late point of the project, when the model was already trained and worked very well)

AttentionReweight on ("destroyed", "abandoned","dirty","rusty","uncrowded","post-apocalyptic","bleached","dry","dead") with a factor of 10

2000 image pairs

	attendances = ["an uncrowded"]
	objects = ["street", "city"]
	seeds = list(range(6000, 7000))

	base_text =  "The scene shows "+a+" "+o
	old_toned_text = ". The setting is bleached-colored, uncrowded, deserted, abandoned post-apocalyptic. The buildings are destroyed, rusty, and trashed. The vegetation is dry and dead."
	mod_text = base_text + old_toned_text

	"cross_replace_steps: 0.1 |self_replace_steps: 0.9 |num_steps: 75 |guidance_scale: 7.5 |controller: AttentionReweight |stable: ldm_stable"

