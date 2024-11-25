---------------------------------------------------------------------------------------------------------------------------------------------------------------
PHONE BOOK 
class Contact {
    String name;
    String phoneNumber;

    Contact(String name, String phoneNumber) {
        this.name = name;
        this.phoneNumber = phoneNumber;
    }
}

class TreeNode {
    Contact contact;
    TreeNode left, right;

    TreeNode(Contact contact) {
        this.contact = contact;
    }
}

class PhoneBook {
    private TreeNode root;

    // Insert a contact
    public void insert(String name, String phoneNumber) {
        root = insertRec(root, new Contact(name, phoneNumber));
    }

    private TreeNode insertRec(TreeNode root, Contact contact) {
        if (root == null) {
            return new TreeNode(contact);
        }
        if (contact.name.compareTo(root.contact.name) < 0) {
            root.left = insertRec(root.left, contact);
        } else if (contact.name.compareTo(root.contact.name) > 0) {
            root.right = insertRec(root.right, contact);
        }
        return root;
    }

    // Search for a contact
    public String search(String name) {
        TreeNode node = searchRec(root, name);
        return node != null ? node.contact.phoneNumber : "Contact not found";
    }

    private TreeNode searchRec(TreeNode root, String name) {
        if (root == null || root.contact.name.equals(name)) {
            return root;
        }
        if (name.compareTo(root.contact.name) < 0) {
            return searchRec(root.left, name);
        }
        return searchRec(root.right, name);
    }

    // Update a contact
    public void update(String name, String newPhoneNumber) {
        TreeNode node = searchRec(root, name);
        if (node != null) {
            node.contact.phoneNumber = newPhoneNumber;
            System.out.println("Contact updated.");
        } else {
            System.out.println("Contact not found.");
        }
    }

    // Delete a contact
    public void delete(String name) {
        root = deleteRec(root, name);
    }

    private TreeNode deleteRec(TreeNode root, String name) {
        if (root == null) {
            return null;
        }
        if (name.compareTo(root.contact.name) < 0) {
            root.left = deleteRec(root.left, name);
        } else if (name.compareTo(root.contact.name) > 0) {
            root.right = deleteRec(root.right, name);
        } else {
            if (root.left == null) return root.right;
            else if (root.right == null) return root.left;

            TreeNode minNode = findMin(root.right);
            root.contact = minNode.contact;
            root.right = deleteRec(root.right, minNode.contact.name);
        }
        return root;
    }

    private TreeNode findMin(TreeNode root) {
        while (root.left != null) {
            root = root.left;
        }
        return root;
    }

    // Display all contacts
    public void display() {
        displayRec(root);
    }

    private void displayRec(TreeNode root) {
        if (root != null) {
            displayRec(root.left);
            System.out.println(root.contact.name + ": " + root.contact.phoneNumber);
            displayRec(root.right);
        }
    }
}

public class PhoneBookApp {
    public static void main(String[] args) {
        PhoneBook phoneBook = new PhoneBook();

        // Inserting contacts
        phoneBook.insert("Alice", "1234567890");
        phoneBook.insert("Bob", "9876543210");
        phoneBook.insert("Charlie", "5555555555");

        // Display contacts
        System.out.println("Phone Book:");
        phoneBook.display();

        // Searching for a contact
        System.out.println("\nSearch for Bob: " + phoneBook.search("Bob"));

        // Updating a contact
        System.out.println("\nUpdating Bob's number...");
        phoneBook.update("Bob", "1111111111");

        // Display contacts after update
        System.out.println("\nPhone Book After Update:");
        phoneBook.display();

        // Deleting a contact
        System.out.println("\nDeleting Alice...");
        phoneBook.delete("Alice");

        // Display contacts after deletion
        System.out.println("\nPhone Book After Deletion:");
        phoneBook.display();
    }
}

----------------------------------------------------------------------------------------------------------------------------------------------------------------------
