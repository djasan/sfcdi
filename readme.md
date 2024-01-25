# Creation Projet avec user pas à pas

## création du projet

- composer create-project symfony/website-skeleton nom_projet
ou
- symfony new --webapp my_project
## éditez votre fichier .env pour y intégrer la BDD

- ```DATABASE_URL="mysql://login:password@127.0.0.1:3306/dbname"```

## creation du user

- ```php bin/console make:user```

## ajout des champs au user

- ```php bin/console make:entity```
-> User

## ajout de l'authentification

- ```php bin/console make:auth```
-> 1
-> Authenticator
-> SecurityController
-> yes

## ajout du crud user

- ```php bin/console make:crud```
-> User

## ajout du registration form

- ```php bin/console make:registration-form```
-> yes
-> no
-> yes

## création de la BDD

- ```php bin/console doctrine:database:create```

## création des autre entities pour mon projet

## creation de leurs crud

## migrations

- ```php bin/console make:migration```
- ```php bin/console doctrine:migrations:migrate```

## création d'une landing page : Home

- je crée un nouveau controller : Home
- ```php bin/console make:controller```
-> HomeController
- Je dois modifier sa route pour qu'elle corresponde à la landing page
- ```@Route("/home", name="app_home")```
- devient :
- ```@Route("/", name="app_home")```
- je peux aussi effectuer une redirection vers un autre controller ( cf UserController new )

## suppression du user new inutil car remplacé par le register

- Dans mon UserController dans la methode new je remplace :

<code><pre>
    /**
     * @Route("/new", name="user_new", methods={"GET","POST"})
     */
    public function new(Request $request): Response
    {
        $user = new User();
        $form = $this->createForm(UserType::class, $user);
        $form->handleRequest($request);

        if ($form->isSubmitted() && $form->isValid()) {
            $entityManager = $this->getDoctrine()->getManager();
            $entityManager->persist($user);
            $entityManager->flush();

            return $this->redirectToRoute('user_index');
        }

        return $this->render('user/new.html.twig', [
            'user' => $user,
            'form' => $form->createView(),
        ]);
    }
</pre></code>

- une redirection vers register
<code><pre>
     
    public function new(Request $request): Response
    {
        return $this->redirectToRoute('app_register');
    }
</pre></code>

## RegistrationFormType : ajout des elements de formulaire qui me manque(ex:fistName,lastName...)  

- ```->add('firstName')``` ou autres

## \Security\Authenticator.php effectuer la redirection après inscription :

<code><pre>
// For example : return new RedirectResponse($this->urlGenerator->generate('some_route'));
        throw new \Exception('TODO: provide a valid redirect inside '.__FILE__);
</pre></code>

- a remplacer par :
 return new RedirectResponse($this->urlGenerator->generate('app_home'));
        // throw new \Exception('TODO: provide a valid redirect inside '.__FILE__);

## pour l'ajout d'un createdAt  : 

- RegistrationController :
    - juste après la validation :
    ```if ($form->isSubmitted() && $form->isValid()) {```
    - ```$created  = new \DateTimeImmutable();$user->setCreatedAt($created);```

## Confirmation du password

- form\RegistrationFormType.php
- remplacement du champ plainPassword :
<code><pre>
->add('password', RepeatedType::class, [
                'type' => PasswordType::class,
                'invalid_message' => 'The password fields must match.',
                'options' => ['attr' => ['class' => 'password-field']],
                'required' => true,
                'first_options'  => ['label' => 'Password'],
                'second_options' => ['label' => 'Repeat Password'],
                'constraints' => [
                    new NotBlank([
                        'message' => 'Please enter a password',
                    ]),
                    new Length([
                        'min' => 6,
                        'minMessage' => 'Your password should be at least {{ limit }} characters',
                        // max length allowed by Symfony for security reasons
                        'max' => 4096,
                    ]),
                ]
            ])
</pre></code>
- ajout du use pour le RepeatedType :
```use Symfony\Component\Form\Extension\Core\Type\RepeatedType;```
- emplacement des plainPassword par password dans RegistrationController
- suppression dans le twig de : ```{{ form_row(registrationForm.plainPassword) }}```
- et ajout dans le meme fichier de  :
```{{ form_row(registrationForm.password.first) }}{{ form_row(registrationForm.password.second) }}```


Projet
Réaliser un site web pour un centre de documentation et d'information (CDI).

Le site web devra afficher sur sa page d'acceuil la liste des livres d'un CDI sous forme de cards par ordre alphabetique de titre de livre , sous cette forme :

Image
Auteur
Titre
Editeur
Permettre à un étudiant de s'inscrire et de se connecter , afficher l'avatar de l'étudiant dans la navbar lorsqu'il est connecté sinon mettre un avatar générique.

Les caractériques d'un étudiant sont :

nom
prénom
email
telpehone
avatar
Permettre à un administrateur :

d'ajouter , modifier , supprimer des étudiants
d'ajouter , modifier , supprimer des livres
d'ajouter , modifier , supprimer des genres de livres
Les caractériques d'un livre sont :

titre
editeur
auteur
isbn
date_publication
image
genre
resume
Les caractériques d'un genre de livre sont :

nom
description
Ce qu'il faut respecter :

Un livre peut avoir plusieurs genres
Gérer le fait qu'un étudiant peut emprunter un livre
Avoir une date de rendu et de sortie pour un livre
Réaliser ce site web avec une installation compléte de Symfony.

Traduire ce projet en language Symfony c'est à dire en créant les bonnes entitées avec leurs relations.

Plusieurs solutions sont possibles pour ce type projet.

Git
Votre travail final se fera dans une branche nommée developp
Vous devrez faire des commits fréquents (atomiques)
Travailler dans des branches , une branche par fonctionnalité
Design
Utiliser aucun framework css , soyer créatif.

Documentation
Documenter dans un readme toute votre démarche.

Retour
Si vous avez des questions , ouvrer une issue sur ce dépôt pour en faire profiter tous le monde.

Have Fun.