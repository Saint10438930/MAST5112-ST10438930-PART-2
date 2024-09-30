# MAST5112-ST10438930-PART-2
import React, { useState } from 'react'; 
import { ScrollView, View, Text, TextInput, Button, StyleSheet, Image, TouchableOpacity } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import { RouteProp } from '@react-navigation/native';

// Sample dish data categorized by course
const sampleDishes = {
  Starters: [
    { name: 'Bruschetta', price: '$6', description: 'Toasted bread with tomato and basil.', image: require('./assets/dishes/bruschetta.jpg') },
    { name: 'Stuffed Mushrooms', price: '$8', description: 'Mushrooms stuffed with cheese and herbs.', image: require('./assets/dishes/stuffed_mushrooms.jpg') },
    { name: 'Caprese Salad', price: '$7', description: 'Fresh mozzarella, tomatoes, and basil.', image: require('./assets/dishes/caprese_salad.jpg') },
    { name: 'Garlic Bread', price: '$5', description: 'Crispy bread with garlic and herbs.', image: require('./assets/dishes/deviled_eggs.jpg') },
  ],
  Appetizers: [
    { name: 'Spring Rolls', price: '$7', description: 'Crispy rolls filled with vegetables.', image: require('./assets/dishes/spring_rolls.jpg') },
    { name: 'Chicken Wings', price: '$10', description: 'Spicy chicken wings with dipping sauce.', image: require('./assets/dishes/chicken_wings.jpg') },
    { name: 'Mini Quiche', price: '$9', description: 'Mini quiches filled with cheese and ham.', image: require('./assets/dishes/mozzarella_sticks.jpg') },
    { name: 'Shrimp Cocktail', price: '$12', description: 'Fresh shrimp with cocktail sauce.', image: require('./assets/dishes/garlic_shrimp.jpg') },
  ],


  MainCourse: [
    { name: 'Grilled Chicken', price: '$15', description: 'Chicken grilled to perfection.', image: require('./assets/dishes/grilled_chicken.jpg') },
    { name: 'Spaghetti Bolognese', price: '$12', description: 'Pasta with a rich meat sauce.', image: require('./assets/dishes/spaghetti_bolognese.jpg') },
    { name: 'Beef Steak', price: '$18', description: 'Juicy beef steak cooked to order.', image: require('./assets/dishes/beef_steak.jpg') },
    { name: 'Vegetarian Lasagna', price: '$14', description: 'Lasagna with roasted vegetables.', image: require('./assets/dishes/vegetarian_lasagna.jpg') },
  ],
  Desserts: [
    { name: 'Chocolate Cake', price: '$5', description: 'Delicious chocolate cake topped with cream.', image: require('./assets/dishes/chocolate_cake.jpg') },
    { name: 'Tiramisu', price: '$6', description: 'Classic Italian coffee-flavored dessert.', image: require('./assets/dishes/tiramisu.jpg') },
    { name: 'Cheesecake', price: '$7', description: 'Creamy cheesecake with berry topping.', image: require('./assets/dishes/cheesecake.jpg') },
    { name: 'Apple Pie', price: '$5', description: 'Warm apple pie with vanilla ice cream.', image: require('./assets/dishes/fruit_tart.jpg') },

  ],
};

const samplePackages = [
  { name: 'Intimate Package', description: 'Perfect for small, romantic dinners.', image: require('./assets/packages/intimate_package.jpg') },
  { name: 'Celebration Package', description: 'Celebrate special moments with flair.', image: require('./assets/packages/celebration_package.jpg') },
  { name: 'Gathering Package', description: 'Ideal for family and friends gatherings.', image: require('./assets/packages/gathering_package.jpg') },
];

// Define the types for navigation params
type RootStackParamList = {
  OpeningScreen: undefined;
  HomeScreen: undefined;
  MenuScreen: undefined;
  CourseDetailScreen: { course: string; dishes: any[] };
  AddMenuItemScreen: { setDishes: any, course: string };
  PackagesScreen: undefined;
  AboutScreen: undefined;
  ContactScreen: undefined;
};

const Stack = createStackNavigator<RootStackParamList>();

// Opening Screen Component
const OpeningScreen = ({ navigation }: any) => {
  return (
    <View style={styles.container}>
      <Image source={require('./assets/Logo.png')} style={styles.logo} />
      <Text style={styles.welcomeText}>Welcome to Chef's Delight</Text>
      <Button title="Get Started" onPress={() => navigation.navigate('HomeScreen')} />
    </View>
  );
};

// Home Screen Component
const HomeScreen = ({ navigation }: any) => {
  return (
    <ScrollView style={styles.container}>
      <Image source={require('./assets/Logo.png')} style={styles.logo} />
      <Text style={styles.heading}>Our Menu</Text>
      <Button title="View Menu" onPress={() => navigation.navigate('MenuScreen')} />
      <Button title="View Packages" onPress={() => navigation.navigate('PackagesScreen')} />
      <Button title="About the Chef" onPress={() => navigation.navigate('AboutScreen')} />
      <Button title="Contact Us" onPress={() => navigation.navigate('ContactScreen')} />
    </ScrollView>
  );
};

// Menu Screen Component
const MenuScreen = ({ navigation }: any) => {
  const [dishes, setDishes] = useState(sampleDishes);
  const courses = ['Starters', 'Appetizers', 'MainCourse', 'Desserts'];

  return (
    <ScrollView style={styles.container}>
      <Text style={styles.heading}>Select a Course</Text>
      {courses.map((course, index) => (
        <TouchableOpacity key={index} style={styles.courseButton} onPress={() => navigation.navigate('CourseDetailScreen', { course, dishes: dishes[course] })}>
          <Text style={styles.courseText}>{course}</Text>
        </TouchableOpacity>
      ))}
      <Button title="Add a Menu Item" onPress={() => navigation.navigate('AddMenuItemScreen', { setDishes, course: 'Starters' })} />
    </ScrollView>
  );
};

// Course Detail Screen
const CourseDetailScreen = ({ route }: RouteProp<RootStackParamList, 'CourseDetailScreen'>) => {
  const { course, dishes } = route.params;

  return (
    <ScrollView style={styles.container}>
      <Text style={styles.heading}>{course}</Text>
      {dishes.map((dish, index) => (
        <View key={index} style={styles.dishContainer}>
          <Image source={dish.image} style={styles.dishImage} />
          <Text style={styles.dishName}>{dish.name}</Text>
          <Text>{dish.description}</Text>
          <Text style={styles.dishPrice}>{dish.price}</Text>
        </View>
      ))}
    </ScrollView>
  );
};

// Add Menu Item Screen
const AddMenuItemScreen = ({ route, navigation }: any) => {
  const { setDishes, course } = route.params;

  const [name, setName] = useState('');
  const [price, setPrice] = useState('');
  const [description, setDescription] = useState('');
  const [imageUri, setImageUri] = useState('');

  const addDish = () => {
    const newDish = { name, price, description, image: { uri: imageUri } };
    setDishes((prevDishes: any) => ({
      ...prevDishes,
      [course]: [...prevDishes[course], newDish],
    }));
    navigation.goBack();
  };

  return (
    <View style={styles.container}>
      <Text style={styles.heading}>Add a New Menu Item</Text>
      <TextInput placeholder="Dish Name" style={styles.input} value={name} onChangeText={setName} />
      <TextInput placeholder="Price" style={styles.input} value={price} onChangeText={setPrice} />
      <TextInput placeholder="Description" style={styles.input} value={description} onChangeText={setDescription} />
      <TextInput placeholder="Image URL" style={styles.input} value={imageUri} onChangeText={setImageUri} />
      <Button title="Add Dish" onPress={addDish} />
    </View>
  );
};

// Packages Screen Component
const PackagesScreen = () => {
  return (
    <ScrollView style={styles.container}>
      <Text style={styles.heading}>Packages</Text>
      {samplePackages.map((pkg, index) => (
        <View key={index} style={styles.packageContainer}>
          <Image source={pkg.image} style={styles.packageImage} />
          <Text style={styles.packageName}>{pkg.name}</Text>
          <Text>{pkg.description}</Text>
        </View>
      ))}
    </ScrollView>
  );
};

// About Screen Component
const AboutScreen = () => {
  return (
    <ScrollView style={styles.container}>
      <Image source={require('./assets/Chef.jpg')} style={styles.chefImage} />
      <Text style={styles.heading}>Meet the Chef</Text>
      <Text style={styles.description}>
        Chef John is a world-class chef known for his exquisite dishes that combine the best of African and international cuisines...
      </Text>
      <View style={styles.socialIcons}>
        <Text>WhatsApp | Instagram | Facebook</Text>
      </View>
    </ScrollView>
  );
};

// Contact Screen Component
const ContactScreen = () => {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [cell, setCell] = useState('');
  const [enquiry, setEnquiry] = useState('');

  return (
    <ScrollView style={styles.container}>
      <Text style={styles.heading}>Contact Us</Text>
      <TextInput placeholder="Your Name" style={styles.input} onChangeText={setName} />
      <TextInput placeholder="Your Email" style={styles.input} onChangeText={setEmail} />
      <TextInput placeholder="Your Cell Number" style={styles.input} onChangeText={setCell} />
      <TextInput placeholder="Your Enquiry" style={styles.input} onChangeText={setEnquiry} multiline />
      <Text style={styles.subheading}>Chef's Available Hours: 9 AM - 9 PM</Text>
      <Text style={styles.subheading}>Chef's Contact: +1234567890 | email@chef.com</Text>
    </ScrollView>
  );
};

// Main App Component
export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="OpeningScreen">
        <Stack.Screen name="OpeningScreen" component={OpeningScreen} />
        <Stack.Screen name="HomeScreen" component={HomeScreen} />
        <Stack.Screen name="MenuScreen" component={MenuScreen} />
        <Stack.Screen name="CourseDetailScreen" component={CourseDetailScreen} />
        <Stack.Screen name="AddMenuItemScreen" component={AddMenuItemScreen} />
        <Stack.Screen name="PackagesScreen" component={PackagesScreen} />
        <Stack.Screen name="AboutScreen" component={AboutScreen} />
        <Stack.Screen name="ContactScreen" component={ContactScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

// Styles
const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#ffffff',
  },
  logo: {
    width: 100,
    height: 100,
    alignSelf: 'center',
    marginBottom: 20,
  },
  welcomeText: {
    fontSize: 24,
    textAlign: 'center',
    marginBottom: 20,
  },
  heading: {
    fontSize: 22,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  courseButton: {
    padding: 10,
    backgroundColor: '#f0f0f0',
    marginVertical: 5,
    borderRadius: 5,
  },
  courseText: {
    fontSize: 18,
  },
  dishContainer: {
    marginVertical: 10,
    padding: 10,
    borderColor: '#ccc',
    borderWidth: 1,
    borderRadius: 5,
  },
  dishImage: {
    width: '100%',
    height: 150,
    borderRadius: 5,
  },
  dishName: {
    fontSize: 18,
    fontWeight: 'bold',
  },
  dishPrice: {
    color: 'green',
    fontWeight: 'bold',
  },
  packageContainer: {
    marginVertical: 10,
    padding: 10,
    borderColor: '#ccc',
    borderWidth: 1,
    borderRadius: 5,
  },
  packageImage: {
    width: '100%',
    height: 150,
    borderRadius: 5,
  },
  packageName: {
    fontSize: 18,
    fontWeight: 'bold',
  },
  chefImage: {
    width: '100%',
    height: 200,
    borderRadius: 5,
  },
  description: {
    marginVertical: 10,
    fontSize: 16,
  },
  socialIcons: {
    flexDirection: 'row',
    justifyContent: 'space-around',
    marginTop: 10,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ccc',
    padding: 10,
    borderRadius: 5,
    marginBottom: 10,
  },
  subheading: {
    fontSize: 16,
    fontWeight: 'bold',
    marginTop: 10,
  },
});
