import com.bigchaindb.builders.BigchainDbConfigBuilder;
import com.bigchaindb.builders.BigchainDbTransactionBuilder;
import com.bigchaindb.constants.Operations;
import com.bigchaindb.model.FulFill;
import com.bigchaindb.model.Transaction;
import domen.AssetData;
import net.i2p.crypto.eddsa.EdDSAPrivateKey;
import net.i2p.crypto.eddsa.EdDSAPublicKey;
import org.junit.Test;

import java.io.IOException;
import java.security.KeyPair;
import java.time.Instant;

public class TransactionTest {

    @Test
    public void test() throws IOException {
        BigchainDbConfigBuilder
                .baseUrl("http://localhost:9984")
                .addToken("app_id", "6f7f4bc2")
                .addToken("app_key", "f1fe8974b9e7ea61dca8f8bb3a626643").setup();

        AssetData assetData = new AssetData("Bob","CREATE","test log 4", Instant.now().toEpochMilli());

        net.i2p.crypto.eddsa.KeyPairGenerator edDsaKpg = new net.i2p.crypto.eddsa.KeyPairGenerator();



        KeyPair aliceKeyPair = edDsaKpg.generateKeyPair();
        KeyPair bobKeyPair = edDsaKpg.generateKeyPair();


         Transaction transaction = BigchainDbTransactionBuilder.init()
                .operation(Operations.CREATE)
                .addAssets(assetData,AssetData.class)
                .buildAndSign((EdDSAPublicKey) aliceKeyPair.getPublic(), (EdDSAPrivateKey) aliceKeyPair.getPrivate())
                .sendTransaction();

        String idPrev = transaction.getId();
        FulFill fulFill = new FulFill();
        fulFill.setOutputIndex(0);
        fulFill.setTransactionId(idPrev);



        Transaction transaction1 = BigchainDbTransactionBuilder.init()
                .operation(Operations.TRANSFER)
                .addInput(null,fulFill,(EdDSAPublicKey) aliceKeyPair.getPublic())
                .buildAndSign((EdDSAPublicKey) bobKeyPair.getPublic(),(EdDSAPrivateKey) aliceKeyPair.getPrivate())
                .sendTransaction();

    }
}
