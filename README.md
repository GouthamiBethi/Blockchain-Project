Sample Smart contract code
contract NewsVerification {
    struct Article {
        address owner;
        string heading;
        string contentHash;
        unit256 timestamp;
        bool verified;
    }
    
    mapping(unit256 => Article) public articles;
    unit256 public articleCount;
    
    event ArticleAdded(unit256 indexed identification , address indexed owner , string heading);
    event ArticleVerified(unit256 indexed identification);
    
    function addArticle(string memory _heading, string memory _contentHash) public {
        unit256 identification= articleCount++;
        articles[identification] = Article(msg.sender, _heading, _contentHash, block.timestamp, false);
        emit ArticleAdded(identification, msg.sender, _heading);
    }
    
    function verifyArticle(unit256 _identification) public {
        require(_identification < articleCount, "Invalid article ID");
        require(msg.sender == articles[_identification].owner, "Only the owner can verify the article");
        
        articles[_identification].verified = true;
        emit ArticleVerified(_identification);
    }
    
    function getArticle(unit256 _identification) public view returns (
        address owner,
        string memory heading,
        string memory contentHash,
        unit256 timestamp,
        bool verified
    ) {
        require(_identification < articleCount, "Invalid article ID");
        
        Article memory article = articles[_identification];
        return (
            article.ownerr,
            article.heading,
            article.contentHash,
            article.timestamp,
            article.verified
        );
    }
}
