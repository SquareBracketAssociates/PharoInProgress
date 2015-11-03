#Magritte

##Description d'un objet Magritte

###Obtenir la description d'un objet Magritte

    Address new magriteDescription

* properties (label du formulaire, classe chargée du rendu, etc.)

* Les différents champs et leur description sont dans children

###Parcourir le contenu d'une description

    | d |
    d := Address new magritteDescription.
    d select: [ :each | #(street place plz canton) includes: each accessor selector ]

###Ajouter dynamiquement une description

    | d |
    d := Address new magritteDescription.
    d copy add:(MAStringDescription new accessor: #email; label: 'E-mail'; priority: 35); yourself.

###Modifier une description existante

    | d |
    d := Address new magritteDescription.
    d collect: [ :each | each accessor selector = #place ifTrue: [ each copy beRequired ] ifFalse: [ each ] ]

###Type de données Magritte

* MABooleanDescription

    <magritteDescription>
    ^ MABooleanDescription new
        checkboxLabel: 'Show comments';
        accessor: #comments;
        componentClass: TBSMagritteCheckboxComponent;
        priority: 700;
        yourself

* MAStringDescription

    ^ MAStringDescription new
        priority: 10;
        label: 'Name';
        accessor: #name;
        beRequired;
        requiredErrorMessage: 'We cannot proceed without a name.';
        comment: 'Please enter your name';
        componentClass: TBSMagritteTextInputComponent;
        yourself

* MAMemoDescription

* MANumberDescription

* MADateDescription

* MADateAndTimeDescription

* MADurationDescription

* MATimeDescription

* MATimeStampDescription

* MASingleOptionDescription

    <magritteDescription>
    ^ MASingleOptionDescription new 
        label: 'Error message styles';
        accessor: #validationMessageStyle;
        options: self validationMessageStyles;
        comment: 'choose an error message style';
        componentClass: MASelectListRowsComponent;
        beRequired;
        yourself


    <magritteDescription>
    ^ MASingleOptionDescription new
        priority: 30;
        label: 'Numbers';
        accessor: #number;
        options: (0 to: 10);
        comment: 'select your favourite number';
        addCondition: [ :v | v ~= 0 ] labelled: 'sorry we are not permitting zero to be favourite';
        addCondition: [ :v | v ~= 1 ] labelled: 'sorry we are not permitting one to be favourite';
        componentClass: TBSMagritteSelectListComponent;
        beRequired;
        yourself

* MAMultipleOptionDescription (permet de sélectionner plusieurs éléments). L'attribut componentClass précise le composant utilisé pour contenir les données (ici une liste).

	<magritteDescription>
	^ MAMultipleOptionDescription new
		priority: 30;
		label: 'Talks';
		accessor: #talks;
		options: MMTalk all;
		reference: MMTalk new magritteDescription;
		componentClass: MAListCompositonComponent;
		comment: 'select the talks you want to schedule';
		beRequired;
		yourself


* MAColorDescription

* MAFileDescription

* MADirectoryDescription

* MAToOneRelationDescription

* MAToManyRelationDescription (construit un tableau)

    ^ MAToManyRelationDescription new
        priority: 50;
        label: 'Attendees';
        accessor: #attendees;
        classes: {MMAttendee};
        comment: 'enter the conference attendees';
        yourself

* MATableDescription

* MATokenDescription

* MAPasswordDescription

* MASymbolDescription

###Afficher un formulaire avec Morphic

| conference |
conference := MMConference demoConferences first.

conference asMagritteMorph addButtons;addWindow;openInWorld

## Utiliser Magritte dans Seaside

###Chargement de BootstrapMagritte

MCHttpRepository
	location: 'http://smalltalkhub.com/mc/TorstenBergmann/BootstrapMagritte/main'
	user: ''
	password: ''

###Construire un composant Seaside

    addressForm := Address new asComponent

###Insérer le composant dans un écran

    html render: (addressForm
        addValidatedForm;
        addMessage: self class label;
        onAnswer: [ :v | self inform: v ];
        yourself)

###Définir le rendu d'un container de composants

    descriptionContainer
        <magritteContainer>
        ^ super descriptionContainer
            componentRenderer: TBSMagritteFormRenderer;
            yourself
###Personnaliser l'apparence d'un formulaire

####Renommer un bouton

    ^ (TBSMagritteExampleFormChooser new asComponent)
        addDecoration: (TBSMagritteFormDecoration buttons: (Array with: #save -> 'Create a new form'));
        yourself

####Ajouter un bouton

    formChooser
    | deco |
	
    deco :=  TBSMagritteFormDecoration buttons: (Array with: #save -> 'Create a new form').
    deco addButton: #hello label: 'Hello'.
	
    ^ (TBSMagritteExampleFormChooser new asComponent)
        addDecoration: deco;
        yourself

#### Ajouter des messages d'erreur à un formulaire

Les messages sont placés au dessus des champs de saisie.

    formChooser isValidationMessageStyleTop ifTrue: [
        theForm addDecoration: TBSMagritteValidationDecoration new ].

#### Spécifier un style CSS et une légende
	
    theForm
        addDecoration: 
            (TBSMagritteFormDecoration newWithDefaultButtons 
                formCss: formChooser layoutStyle;
                legend: formChooser optionsSelectedString;
                yourself);
        yourself.

Les styles possibles pour le formulaire sont:

* "form-vertical"

* "form-inline"

* "form-horizontal"

###Composants Bootstrap adaptés à Magritte

* TBSListCompositionComponent

* TBSMagritteCheckboxComponent

* TBSMagritteCheckboxGroupComponent

* TBSMagritteDisabledTextInputComponent

* TBSMagritteFormDecoration

* TBSMagritteFormRenderer

* TBSMagritteOneToManyComponent

* TBSMagritteRadioGroupComponent

* TBSMagritteReport

    theReport := TBSMagritteReport rows: self demoRows description: TBSMagritteExampleFormDescription new magritteDescription. 

* TBSMagritteTextAreaComponent

* TBSMagritteTextInputComponent

* TBSMagritteValidationDecoration

* TBSVerifiedPasswordComponent

###Exemples d'utilisation de saisie par Magritte

####Utilisation de boutons radio

    <magritteDescription>
    ^ MASingleOptionDescription new 
        label: 'Layout styles';
        accessor: #layoutStyle;
        options: self layoutStyles;
        comment: 'choose a layout style';
        componentClass: TBSMagritteRadioGroupComponent;
        beRequired;
        yourself

####Utilisation d'une case à cocher

    <magritteDescription>
    ^ MABooleanDescription new
        checkboxLabel: 'Show comments';
        accessor: #comments;
        componentClass: TBSMagritteCheckboxComponent;
        priority: 700;
        yourself

####Sélection d'un élément dans une liste

    <magritteDescription>
    ^ MASingleOptionDescription new 
        label: 'Error message styles';
        accessor: #validationMessageStyle;
        options: self validationMessageStyles;
        comment: 'choose an error message style';
        componentClass: MASelectListRowsComponent;
        beRequired;

##Seaside

###Ajouter les bibliothèques JQuery et Bootstrap au chargement d'un composant

    updateRoot: anHtmlRoot
        super updateRoot: anHtmlRoot.
	
        JQDevelopmentLibrary default updateRoot: anHtmlRoot.
        TBSDevelopmentLibrary default updateRoot: anHtmlRoot.
	
        anHtmlRoot title: 'Seaside Bootstrap examples'

##Bootstrap

###Boites d'alerte

####Succès de l'opération

    self informSuccess: [ :r | r strong: 'Well done'; text: ' you have managed to survive this sample''s arbitary validation.' ]

####Echec d'une opération

    self informWarning: [ :r | r strong: 'Cancelled'; text: ' Oh well you can always try again to see if you can work-through this sample''s arbitary validation.' ] 

##Exemples d'utilisation de Magritte

###Pier

    MCHttpRepository
        location: 'http://smalltalkhub.com/mc/Pier/Pier3/main'
        user: ''
        password: ''

###MagritteMagic

    MCGemstoneRepository
        location: 'http://ss3.gemstone.com/ss/MagritteMagic'
        user: ''
        password: ''

##Informations sur Magritte

https://www.youtube.com/watch?v=mKZUhPNWFyI